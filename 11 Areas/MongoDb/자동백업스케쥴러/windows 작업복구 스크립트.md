mongodb_restore.sh
```
#!/bin/bash

# MongoDB 복구 스크립트 (Windows 버전)
# 사용법: ./mongodb_restore.sh [백업파일경로] [대상데이터베이스명]

# ===========================================
# 설정 섹션 - 필요에 따라 수정하세요
# ===========================================

# MongoDB 설정
MONGO_HOST="localhost"
MONGO_PORT="27017"
MONGO_USER=""  # 인증이 필요한 경우 사용자명
MONGO_PASS=""  # 인증이 필요한 경우 비밀번호

# 백업 디렉토리 설정 (Windows 경로)
BACKUP_DIR="C:/mongodb_backups"
LOG_DIR="C:/mongodb_backups/logs"
TEMP_DIR="C:/mongodb_backups/temp"

# mongorestore 경로 (MongoDB 설치 경로에 따라 수정)
MONGORESTORE_PATH="/c/Program Files/MongoDB/Server/7.0/bin/mongorestore.exe"

# ===========================================
# 함수 정의
# ===========================================

# 로그 함수
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_DIR/restore_$(date '+%Y%m%d').log"
}

# 사용법 표시
show_usage() {
    echo "사용법:"
    echo "  $0 [백업파일경로] [대상데이터베이스명]"
    echo ""
    echo "예시:"
    echo "  $0 backup_mystore_20241201_020000.tar.gz mystore_restored"
    echo "  $0                                       # 대화형 모드"
    echo ""
    echo "옵션:"
    echo "  -l, --list     : 사용 가능한 백업 파일 목록 표시"
    echo "  -h, --help     : 도움말 표시"
}

# 백업 파일 목록 표시
list_backups() {
    echo "사용 가능한 백업 파일:"
    echo "======================================"
    
    local count=0
    for backup_file in "$BACKUP_DIR"/*.tar.gz; do
        if [ -f "$backup_file" ]; then
            count=$((count + 1))
            local file_size=$(du -h "$backup_file" | cut -f1)
            local file_date=$(stat -c %y "$backup_file" 2>/dev/null || stat -f "%Sm" "$backup_file")
            echo "$count. $(basename "$backup_file")"
            echo "   크기: $file_size"
            echo "   날짜: $file_date"
            echo ""
        fi
    done
    
    if [ $count -eq 0 ]; then
        echo "백업 파일이 없습니다."
        echo "백업 디렉토리: $BACKUP_DIR"
    fi
}

# 디렉토리 생성 함수
create_directories() {
    if [ ! -d "$LOG_DIR" ]; then
        mkdir -p "$LOG_DIR"
        log "로그 디렉토리 생성: $LOG_DIR"
    fi
    
    if [ ! -d "$TEMP_DIR" ]; then
        mkdir -p "$TEMP_DIR"
        log "임시 디렉토리 생성: $TEMP_DIR"
    fi
}

# MongoDB 연결 확인
check_mongodb_connection() {
    log "MongoDB 연결 확인 중..."
    
    if command -v mongo &> /dev/null; then
        MONGO_CMD="mongo"
    elif command -v mongosh &> /dev/null; then
        MONGO_CMD="mongosh"
    else
        log "ERROR: mongo 또는 mongosh 명령어를 찾을 수 없습니다."
        return 1
    fi
    
    # MongoDB 연결 테스트
    if $MONGO_CMD --host $MONGO_HOST:$MONGO_PORT --eval "db.adminCommand('ismaster')" &> /dev/null; then
        log "MongoDB 연결 성공"
        return 0
    else
        log "ERROR: MongoDB에 연결할 수 없습니다."
        return 1
    fi
}

# 백업 파일 검증
validate_backup_file() {
    local backup_file="$1"
    
    if [ ! -f "$backup_file" ]; then
        log "ERROR: 백업 파일을 찾을 수 없습니다: $backup_file"
        return 1
    fi
    
    # 파일 확장자 확인
    if [[ ! "$backup_file" =~ \.tar\.gz$ ]]; then
        log "ERROR: 유효하지 않은 백업 파일 형식입니다. .tar.gz 파일이어야 합니다."
        return 1
    fi
    
    # 파일 무결성 확인
    if ! tar -tzf "$backup_file" &> /dev/null; then
        log "ERROR: 백업 파일이 손상되었습니다: $backup_file"
        return 1
    fi
    
    log "백업 파일 검증 완료: $backup_file"
    return 0
}

# 백업 파일 압축 해제
extract_backup() {
    local backup_file="$1"
    local extract_dir="$TEMP_DIR/$(basename "$backup_file" .tar.gz)"
    
    log "백업 파일 압축 해제 중: $backup_file"
    
    # 기존 임시 디렉토리 삭제
    if [ -d "$extract_dir" ]; then
        rm -rf "$extract_dir"
    fi
    
    # 압축 해제
    if tar -xzf "$backup_file" -C "$TEMP_DIR"; then
        log "압축 해제 완료: $extract_dir"
        echo "$extract_dir"
        return 0
    else
        log "ERROR: 압축 해제 실패"
        return 1
    fi
}

# 데이터베이스 복구 실행
perform_restore() {
    local backup_dir="$1"
    local target_db="$2"
    local drop_existing="$3"
    
    log "데이터베이스 복구 시작"
    log "백업 디렉토리: $backup_dir"
    log "대상 데이터베이스: $target_db"
    
    # mongorestore 명령어 구성
    local mongorestore_cmd="\"$MONGORESTORE_PATH\" --host $MONGO_HOST:$MONGO_PORT"
    
    if [ -n "$MONGO_USER" ] && [ -n "$MONGO_PASS" ]; then
        mongorestore_cmd="$mongorestore_cmd --username $MONGO_USER --password $MONGO_PASS"
    fi
    
    # 기존 데이터베이스 삭제 옵션
    if [ "$drop_existing" = "yes" ]; then
        mongorestore_cmd="$mongorestore_cmd --drop"
        log "기존 데이터베이스를 삭제하고 복구합니다."
    fi
    
    # 데이터베이스 이름 변경 (nsFrom, nsTo 사용)
    local original_db=$(find "$backup_dir" -maxdepth 1 -type d ! -path "$backup_dir" | head -1 | xargs basename)
    if [ -n "$original_db" ] && [ "$original_db" != "$target_db" ]; then
        mongorestore_cmd="$mongorestore_cmd --nsFrom \"$original_db.*\" --nsTo \"$target_db.*\""
        log "데이터베이스 이름 변경: $original_db -> $target_db"
    fi
    
    mongorestore_cmd="$mongorestore_cmd \"$backup_dir\""
    
    # 복구 실행
    log "복구 명령어 실행 중..."
    if eval $mongorestore_cmd; then
        log "데이터베이스 복구 완료: $target_db"
        return 0
    else
        log "ERROR: 데이터베이스 복구 실패"
        return 1
    fi
}

# 복구 후 정리
cleanup() {
    log "임시 파일 정리 중..."
    if [ -d "$TEMP_DIR" ]; then
        find "$TEMP_DIR" -type f -mmin +60 -delete 2>/dev/null
        find "$TEMP_DIR" -type d -empty -delete 2>/dev/null
    fi
    log "정리 완료"
}

# 대화형 모드
interactive_mode() {
    echo "MongoDB 데이터베이스 복구 (대화형 모드)"
    echo "======================================"
    
    # 백업 파일 선택
    list_backups
    echo ""
    read -p "복구할 백업 파일명을 입력하세요 (확장자 포함): " backup_filename
    
    if [ -z "$backup_filename" ]; then
        echo "백업 파일명이 입력되지 않았습니다."
        exit 1
    fi
    
    local backup_file="$BACKUP_DIR/$backup_filename"
    
    # 대상 데이터베이스명 입력
    read -p "대상 데이터베이스명을 입력하세요: " target_db
    if [ -z "$target_db" ]; then
        echo "데이터베이스명이 입력되지 않았습니다."
        exit 1
    fi
    
    # 기존 데이터 삭제 여부
    read -p "기존 데이터베이스를 삭제하고 복구하시겠습니까? (y/n): " drop_choice
    if [[ "$drop_choice" =~ ^[Yy]$ ]]; then
        drop_existing="yes"
    else
        drop_existing="no"
    fi
    
    # 확인
    echo ""
    echo "복구 설정 확인:"
    echo "- 백업 파일: $backup_file"
    echo "- 대상 DB: $target_db"
    echo "- 기존 데이터 삭제: $drop_existing"
    echo ""
    read -p "복구를 진행하시겠습니까? (y/n): " confirm
    
    if [[ ! "$confirm" =~ ^[Yy]$ ]]; then
        echo "복구가 취소되었습니다."
        exit 0
    fi
    
    # 복구 실행
    restore_database "$backup_file" "$target_db" "$drop_existing"
}

# 메인 복구 함수
restore_database() {
    local backup_file="$1"
    local target_db="$2"
    local drop_existing="$3"
    
    # 백업 파일 검증
    if ! validate_backup_file "$backup_file"; then
        return 1
    fi
    
    # 백업 파일 압축 해제
    local extract_dir
    if ! extract_dir=$(extract_backup "$backup_file"); then
        return 1
    fi
    
    # 복구 실행
    if perform_restore "$extract_dir" "$target_db" "$drop_existing"; then
        log "복구 완료"
    else
        log "복구 실패"
        return 1
    fi
    
    # 정리
    cleanup
}

# ===========================================
# 메인 실행부
# ===========================================

main() {
    # 명령행 인자 처리
    case "$1" in
        -l|--list)
            list_backups
            exit 0
            ;;
        -h|--help)
            show_usage
            exit 0
            ;;
    esac
    
    log "=========================================="
    log "MongoDB 복구 스크립트 시작"
    log "=========================================="
    
    # 디렉토리 생성
    create_directories
    
    # MongoDB 연결 확인
    if ! check_mongodb_connection; then
        log "복구 중단: MongoDB 연결 실패"
        exit 1
    fi
    
    # 인자에 따른 모드 선택
    if [ $# -eq 0 ]; then
        # 대화형 모드
        interactive_mode
    elif [ $# -eq 2 ]; then
        # 명령행 모드
        local backup_file="$1"
        local target_db="$2"
        
        # 상대 경로인 경우 절대 경로로 변환
        if [[ ! "$backup_file" = /* ]]; then
            if [ -f "$BACKUP_DIR/$backup_file" ]; then
                backup_file="$BACKUP_DIR/$backup_file"
            elif [ ! -f "$backup_file" ]; then
                log "ERROR: 백업 파일을 찾을 수 없습니다: $backup_file"
                exit 1
            fi
        fi
        
        read -p "기존 데이터베이스를 삭제하고 복구하시겠습니까? (y/n): " drop_choice
        if [[ "$drop_choice" =~ ^[Yy]$ ]]; then
            drop_existing="yes"
        else
            drop_existing="no"
        fi
        
        restore_database "$backup_file" "$target_db" "$drop_existing"
    else
        echo "ERROR: 잘못된 인자 개수"
        show_usage
        exit 1
    fi
    
    log "=========================================="
    log "MongoDB 복구 스크립트 완료"
    log "=========================================="
}

# 스크립트 실행
main "$@"
```

## MongoDB 백업 복구 방법:

### 1. 복구 스크립트 사용법

#### **대화형 모드 (권장)**

bash

```bash
cd /c/scripts
./mongodb_restore.sh
```

- 백업 파일 목록이 표시됩니다
- 복구할 파일과 대상 데이터베이스를 선택할 수 있습니다

#### **명령행 모드**

bash

```bash
# 사용법: ./mongodb_restore.sh [백업파일] [대상DB명]
./mongodb_restore.sh backup_mystore_20241201_020000.tar.gz mystore_restored
```

#### **백업 파일 목록 확인**

bash

```bash
./mongodb_restore.sh --list
```

### 2. 수동 복구 방법 (mongorestore 직접 사용)

#### **Step 1: 백업 파일 압축 해제**

bash

```bash
cd /c/mongodb_backups
tar -xzf backup_mystore_20241201_020000.tar.gz
```

#### **Step 2: mongorestore 실행**

bash

```bash
# 기본 복구 (같은 데이터베이스명)
"/c/Program Files/MongoDB/Server/7.0/bin/mongorestore.exe" --host localhost:27017 backup_mystore_20241201_020000/

# 다른 데이터베이스명으로 복구
"/c/Program Files/MongoDB/Server/7.0/bin/mongorestore.exe" --host localhost:27017 --nsFrom "mystore.*" --nsTo "mystore_restored.*" backup_mystore_20241201_020000/

# 기존 데이터 삭제 후 복구
"/c/Program Files/MongoDB/Server/7.0/bin/mongorestore.exe" --host localhost:27017 --drop backup_mystore_20241201_020000/
```

### 3. 복구 옵션 설명

|옵션|설명|
|---|---|
|`--drop`|복구 전에 기존 컬렉션을 삭제|
|`--nsFrom`|원본 네임스페이스 지정|
|`--nsTo`|대상 네임스페이스 지정|
|`--db`|특정 데이터베이스만 복구|
|`--collection`|특정 컬렉션만 복구|
|`--host`|MongoDB 서버 주소|
|`--port`|MongoDB 포트|
|`--username`|인증 사용자명|
|`--password`|인증 비밀번호|

### 4. 복구 시나리오별 예시

#### **전체 데이터베이스 복구**

bash

```bash
./mongodb_restore.sh backup_mystore_20241201_020000.tar.gz mystore
```

#### **다른 이름으로 복구 (테스트용)**

bash

```bash
./mongodb_restore.sh backup_mystore_20241201_020000.tar.gz mystore_test
```

#### **특정 컬렉션만 복구**

bash

```bash
"/c/Program Files/MongoDB/Server/7.0/bin/mongorestore.exe" --host localhost:27017 --db mystore --collection users backup_mystore_20241201_020000/mystore/users.bson
```

### 5. 복구 전 체크리스트

1. **MongoDB 서비스 실행 확인**

bash

```bash
   # Windows에서 서비스 상태 확인
   sc query MongoDB
```

2. **백업 파일 무결성 확인**

bash

```bash
   tar -tzf backup_mystore_20241201_020000.tar.gz
```

3. **충분한 디스크 공간 확인**
    - 백업 파일 크기의 2-3배 여유공간 필요
4. **기존 데이터 백업** (중요!)
    - 복구 전에 현재 데이터를 백업해두세요

### 6. 복구 후 검증

#### **데이터 확인**

javascript

```javascript
// MongoDB 콘솔에서 확인
use mystore_restored
db.stats()
show collections
db.users.countDocuments()
```

#### **인덱스 확인**

javascript

```javascript
db.users.getIndexes()
```

### 7. 문제 해결

#### **권한 오류 시**

bash

```bash
# 스크립트 실행 권한 부여
chmod +x /c/scripts/mongodb_restore.sh
```

#### **경로 오류 시**

- MongoDB 설치 경로 확인 후 스크립트의 `MONGORESTORE_PATH` 수정

#### **메모리 부족 시**

bash

```bash
# 작은 배치 크기로 복구
mongorestore --batchSize 100 [기타옵션]
```

이 복구 스크립트를 사용하면 안전하고 체계적으로 MongoDB 백업을 복구할 수 있습니다.