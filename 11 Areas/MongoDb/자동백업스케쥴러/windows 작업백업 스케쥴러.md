mongodb_backup.sh
```
#!/bin/bash

# MongoDB 백업 스크립트 (Windows 버전)
# 사용법: ./mongodb_backup.sh

# ===========================================
# 설정 섹션 - 필요에 따라 수정하세요
# ===========================================

# MongoDB 설정
MONGO_HOST="localhost"
MONGO_PORT="27017"
DATABASE_NAME="your_database_name"  # 백업할 데이터베이스 이름
MONGO_USER=""  # 인증이 필요한 경우 사용자명
MONGO_PASS=""  # 인증이 필요한 경우 비밀번호

# 백업 디렉토리 설정 (Windows 경로)
BACKUP_DIR="C:/mongodb_backups"
LOG_DIR="C:/mongodb_backups/logs"

# 백업 파일 보관 기간 (일)
RETENTION_DAYS=30

# mongodump 경로 (MongoDB 설치 경로에 따라 수정)
MONGODUMP_PATH="/c/Program Files/MongoDB/Tools/100/bin/mongodump.exe"

# ===========================================
# 함수 정의
# ===========================================

# 로그 함수
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_DIR/backup_$(date '+%Y%m%d').log"
}

# 디렉토리 생성 함수
create_directories() {
    if [ ! -d "$BACKUP_DIR" ]; then
        mkdir -p "$BACKUP_DIR"
        log "백업 디렉토리 생성: $BACKUP_DIR"
    fi
    
    if [ ! -d "$LOG_DIR" ]; then
        mkdir -p "$LOG_DIR"
        log "로그 디렉토리 생성: $LOG_DIR"
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

# 백업 실행 함수
perform_backup() {
    local timestamp=$(date '+%Y%m%d_%H%M%S')
    local backup_path="$BACKUP_DIR/backup_${DATABASE_NAME}_${timestamp}"
    
    log "백업 시작: $DATABASE_NAME"
    log "백업 경로: $backup_path"
    
    # mongodump 명령어 구성
    local mongodump_cmd="\"$MONGODUMP_PATH\" --host $MONGO_HOST:$MONGO_PORT"
    
    if [ -n "$DATABASE_NAME" ]; then
        mongodump_cmd="$mongodump_cmd --db $DATABASE_NAME"
    fi
    
    if [ -n "$MONGO_USER" ] && [ -n "$MONGO_PASS" ]; then
        mongodump_cmd="$mongodump_cmd --username $MONGO_USER --password $MONGO_PASS"
    fi
    
    mongodump_cmd="$mongodump_cmd --out \"$backup_path\""
    
    # 백업 실행
    if eval $mongodump_cmd; then
        log "백업 완료: $backup_path"
        
        # 백업 파일 압축
        cd "$BACKUP_DIR"
        if tar -czf "backup_${DATABASE_NAME}_${timestamp}.tar.gz" "backup_${DATABASE_NAME}_${timestamp}"; then
            log "백업 파일 압축 완료: backup_${DATABASE_NAME}_${timestamp}.tar.gz"
            # 압축 후 원본 디렉토리 삭제
            rm -rf "backup_${DATABASE_NAME}_${timestamp}"
            log "원본 백업 디렉토리 삭제"
        else
            log "ERROR: 백업 파일 압축 실패"
        fi
    else
        log "ERROR: 백업 실패"
        return 1
    fi
}

# 오래된 백업 파일 삭제
cleanup_old_backups() {
    log "오래된 백업 파일 정리 중... (${RETENTION_DAYS}일 이상)"
    
    find "$BACKUP_DIR" -name "backup_*.tar.gz" -type f -mtime +$RETENTION_DAYS -delete
    find "$LOG_DIR" -name "backup_*.log" -type f -mtime +$RETENTION_DAYS -delete
    
    log "오래된 백업 파일 정리 완료"
}

# 백업 상태 확인
check_backup_status() {
    local latest_backup=$(find "$BACKUP_DIR" -name "backup_*.tar.gz" -type f -printf '%T@ %p\n' 2>/dev/null | sort -n | tail -1 | cut -d' ' -f2-)
    
    if [ -n "$latest_backup" ]; then
        local backup_size=$(du -h "$latest_backup" | cut -f1)
        log "최신 백업 파일: $(basename "$latest_backup") (크기: $backup_size)"
    else
        log "백업 파일이 없습니다."
    fi
}

# ===========================================
# 메인 실행부
# ===========================================

main() {
    log "=========================================="
    log "MongoDB 백업 스크립트 시작"
    log "=========================================="
    
    # 디렉토리 생성
    create_directories
    
    # MongoDB 연결 확인
    if ! check_mongodb_connection; then
        log "백업 중단: MongoDB 연결 실패"
        exit 1
    fi
    
    # 백업 실행
    if perform_backup; then
        log "백업 성공"
    else
        log "백업 실패"
        exit 1
    fi
    
    # 오래된 백업 정리
    cleanup_old_backups
    
    # 백업 상태 확인
    check_backup_status
    
    log "=========================================="
    log "MongoDB 백업 스크립트 완료"
    log "=========================================="
}
# 스크립트 실행
main "$@"

```


mongodb_backup_scheduler.bat
```
@echo off
REM MongoDB 백업 스케줄러 (Windows 배치 파일)
REM Windows Task Scheduler에서 사용하기 위한 래퍼 스크립트

REM ===========================================
REM 설정 섹션
REM ===========================================

REM Git Bash 경로 (설치 경로에 따라 수정)
SET BASH_PATH="C:\Program Files\Git\bin\bash.exe"

REM 백업 스크립트 경로
SET SCRIPT_PATH="C:\scripts\mongodb_backup.sh"

REM 로그 파일 경로
SET LOG_PATH="C:\mongodb_backups\logs\scheduler.log"

REM ===========================================
REM 실행부
REM ===========================================

echo [%date% %time%] MongoDB 백업 스케줄러 시작 >> %LOG_PATH%

REM bash 스크립트 실행
%BASH_PATH% %SCRIPT_PATH%

IF %ERRORLEVEL% EQU 0 (
    echo [%date% %time%] 백업 성공 >> %LOG_PATH%
) ELSE (
    echo [%date% %time%] 백업 실패 - 오류 코드: %ERRORLEVEL% >> %LOG_PATH%
)

echo [%date% %time%] MongoDB 백업 스케줄러 종료 >> %LOG_PATH%
echo. >> %LOG_PATH%
```


## 설정 및 사용 방법:
[MongoDb Tool 설치](https://www.mongodb.com/ko-kr/docs/database-tools/installation/installation-windows/)
### 1. 스크립트 설정

1. `mongodb_backup.sh` 파일의 설정 섹션을 수정:
    - `DATABASE_NAME`: 백업할 데이터베이스 이름
    - `MONGO_USER`, `MONGO_PASS`: 인증이 필요한 경우
    - `BACKUP_DIR`: 백업 파일 저장 경로
    - `MONGODUMP_PATH`: MongoDB 설치 경로

### 2. 파일 저장

- `mongodb_backup.sh`를 `C:\scripts\` 폴더에 저장
- `mongodb_backup_scheduler.bat`를 같은 폴더에 저장

### 3. Windows Task Scheduler 설정

1. **작업 스케줄러** 열기 (Windows 검색에서 "[Task Scheduler](https://m.blog.naver.com/toruin84/222545800188)")
2. **기본 작업 만들기** 클릭
3. 작업 이름: "MongoDB Backup"
4. **트리거** 설정:
    - 매일: 매일 특정 시간 (예: 새벽 2시)
    - 매주: 매주 특정 요일
    - 매월: 매월 특정 날짜
5. **동작** 설정:
    - 프로그램 시작: `C:\scripts\mongodb_backup_scheduler.bat`

### 4. 권한 설정

bash

```bash
# Git Bash에서 실행 권한 부여
chmod +x /c/scripts/mongodb_backup.sh
```

### 5. 수동 테스트

bash

```bash
# Git Bash에서 테스트 실행
cd /c/scripts
./mongodb_backup.sh
```

## 주요 기능:

- **자동 백업**: mongodump를 사용한 완전한 데이터베이스 백업
- **압축**: 백업 파일을 tar.gz로 압축하여 저장공간 절약
- **로그 관리**: 상세한 로그 기록
- **자동 정리**: 설정된 기간보다 오래된 백업 파일 자동 삭제
- **오류 처리**: MongoDB 연결 실패 시 백업 중단

## 백업 파일 형식:

- `backup_[데이터베이스명]_[YYYYMMDD_HHMMSS].tar.gz`
- 예: `backup_mystore_20241201_020000.tar.gz`

이 스크립트는 Windows 환경에서 MongoDB Local 인스턴스의 정기적인 백업을 자동화해줍니다.