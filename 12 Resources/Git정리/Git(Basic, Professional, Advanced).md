## Git Basic

### 1️⃣ Git 개념

- Git은 **분산 버전 관리 시스템(DVCS)**
    
- **로컬 저장소, 원격 저장소**로 구성됨
    

### 2️⃣ Git 기본 명령어

```
git init  # 저장소 초기화
git clone <URL>  # 원격 저장소 복제
git status  # 현재 상태 확인
git add <파일명>  # 변경 사항 스테이징
git commit -m "설명"  # 커밋 생성
git push origin main  # 원격 저장소에 반영
git pull origin main  # 최신 변경 사항 가져오기
```

### 3️⃣ Git 브랜치

```
git branch <브랜치명>  # 새 브랜치 생성
git checkout <브랜치명>  # 브랜치 전환
git merge <브랜치명>  # 브랜치 병합
git rebase <브랜치명>  # Rebase 수행
git branch -d <브랜치명>  # 브랜치 삭제
```

---

## 🚀 Git Professional

### 1️⃣ Git 브랜치 전략

#### 🔹 Git Flow

- `main`: 배포용 안정 브랜치
    
- `develop`: 개발 브랜치
    
- `feature/*`: 기능 개발 브랜치
    
- `release/*`: 배포 전 테스트 브랜치
    
- `hotfix/*`: 긴급 수정 브랜치
    

```
git checkout -b feature/new-feature develop
git checkout develop
git merge feature/new-feature
git checkout main
git merge develop
git push origin main
```

#### 🔹 GitHub Flow

- `main` 브랜치에서 직접 개발, feature 브랜치 활용
    

```
git checkout -b feature/new-feature
git commit -m "New feature"
git push origin feature/new-feature
```

### 2️⃣ Git Rebase & Squash

```
git rebase main  # 최신 커밋으로 재정렬
git rebase -i HEAD~3  # 최근 3개 커밋 합치기(Squash)
```

### 3️⃣ Git Stash (작업 임시 저장)

```
git stash  # 변경 사항 임시 저장
git stash pop  # 마지막으로 저장한 변경 사항 복원
```

### 4️⃣ Git Cherry-Pick (특정 커밋만 선택 적용)

```
git cherry-pick <커밋 해시>
```

### 5️⃣ Git Hooks (자동화)

```
# .git/hooks/pre-commit
#!/bin/sh
npm test  # 커밋 전에 테스트 실행
chmod +x .git/hooks/pre-commit
```

---

## 🏆 Git Advanced

### 1️⃣ Git 내부 구조

- **3가지 주요 저장소**: Working Directory / Staging Area (Index) / Repository
    
- **4가지 주요 객체**: Blob, Tree, Commit, Tag
    

```
git cat-file -t <객체 해시>  # 객체 유형 확인
git cat-file -p <객체 해시>  # 객체 내용 확인
```

### 2️⃣ Git Bisect (버그 찾기)

```
git bisect start
git bisect bad HEAD  # 버그 발생 커밋
git bisect good <커밋 해시>  # 정상 작동 커밋
git bisect reset
```

### 3️⃣ Git Submodule (서브모듈)

```
git submodule add <서브모듈 URL> <경로>
git submodule update --init --recursive
```

### 4️⃣ Git Worktree (멀티 브랜치 작업)

```
git worktree add ../new-branch new-branch
```

### 5️⃣ Git Blame & Pickaxe (변경 추적)

```
git blame <파일명>
git log -S "특정 문자열"
```

### 6️⃣ Git Reset vs. Revert

|명령어|설명|
|---|---|
|`git reset --soft HEAD~1`|마지막 커밋 취소 (파일 유지)|
|`git reset --mixed HEAD~1`|커밋 + 스테이징 취소|
|`git reset --hard HEAD~1`|커밋 + 파일 삭제 (복구 불가)|
|`git revert <커밋 해시>`|특정 커밋 취소 (새로운 커밋 생성)|

### 7️⃣ Git LFS (대용량 파일 관리)

```
git lfs install
git lfs track "*.psd"
```

### 8️⃣ Git Garbage Collection & Performance

```
git gc  # 불필요한 객체 정리
git prune  # 참조되지 않는 객체 삭제
git fsck  # Git 저장소 무결성 검사
```

### 9️⃣ Git Credential Store (로그인 자동화)

```
git config --global credential.helper store
```

### 🔟 삭제된 브랜치 복구

```
git reflog  # 최근 HEAD 이동 기록 확인
git checkout -b <새 브랜치명> <커밋 해시>
git push origin <새 브랜치명>  # 원격 저장소에 다시 업로드 (선택)
```

### 🔟 Git Log --oneline

```
git log --oneline --grep="검색어"
git log --oneline -5  # 최근 5개 커밋 출력
git log --oneline --graph --all --decorate
```

### 🔎 Git Log Grep (특정 문자열 검색)

```
git log --grep="검색어"  # 커밋 메시지에서 특정 키워드 검색
git log --grep="검색어" -i  # 대소문자 구분 없이 검색
git log --grep="버그 수정" --grep="기능 추가"  # OR 조건 검색
git log --grep="버그" --grep="수정" --all-match  # AND 조건 검색
git log -S "검색어" -- <파일명>  # 특정 파일 내 변경 검색
git log --oneline --grep="검색어"  # 한 줄 요약 형태로 검색 결과 출력
```

---

## 🎯 결론

✔️ Git 기본 개념 및 주요 명령어 숙지 ✔️ Git Flow & GitHub Flow 전략 활용 ✔️ Rebase, Squash, Cherry-Pick로 히스토리 정리 ✔️ Git Bisect, Worktree, Hooks 등 고급 기능 활용 ✔️ LFS 및 성능 최적화 기법 적용

![[Pasted image 20250325174052.png]]

🚀 **Obsidian에서 [[Git Advanced]] 노트와 연결하여 활용 가능!**

🔎 Git 전략 [Git 관리 하는 방법](https://youtu.be/tkkbYCajCjM?si=UBkFRN4hZe8Vj0Bd)
```
```