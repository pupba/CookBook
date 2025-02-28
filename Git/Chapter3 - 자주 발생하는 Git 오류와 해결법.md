
## 자주 발생하는 Git 오류와 해결 방법

Git을 사용하다 보면 여러 가지 문제가 발생할 수 있습니다. 이번 챕터에서는 신입 개발자가 자주 마주치는 Git 오류와 해결 방법을 알아보겠습니다.

### 1. "fatal: not a git repository" 오류

이 오류는 Git 저장소가 아닌 디렉토리에서 Git 명령어를 실행할 때 발생합니다.

**해결 방법**:

```bash
# 현재 디렉토리에 Git 저장소 초기화
git init

# 또는 올바른 Git 저장소 디렉토리로 이동
cd 프로젝트폴더
```


### 2. 병합 충돌(Merge Conflicts)

여러 개발자가 같은 파일의 같은 부분을 수정했을 때 발생합니다.

**해결 방법**:

1. 충돌이 발생한 파일을 열어 충돌 부분 확인 (<<<<<, =====, >>>>> 마커로 표시됨)
2. 코드를 수정하여 충돌 해결
3. 변경사항 저장 후 커밋
```bash
git add .
git commit -m "충돌 해결"
```


### 3. "non-fast-forward" 오류

원격 저장소에 이미 다른 커밋이 있을 때 발생합니다.

**해결 방법**:

```bash
# 원격 변경사항 가져온 후 푸시
git pull origin 브랜치명
git push origin 브랜치명

# 또는 rebase 사용
git pull --rebase origin 브랜치명
git push origin 브랜치명
```


### 4. 로컬 변경사항 취소하기

파일 수정을 취소하고 싶을 때 사용합니다.

**해결 방법**:

```bash
# 특정 파일의 변경사항 취소
git checkout -- 파일명

# 모든 변경사항 취소
git checkout -- .
```


### 5. 커밋 실수 수정하기

방금 한 커밋을 수정하고 싶을 때 사용합니다.

**해결 방법**:

```bash
# 마지막 커밋 메시지 수정
git commit --amend -m "새로운 커밋 메시지"

# 파일 추가 후 마지막 커밋 수정
git add 빠진파일
git commit --amend --no-edit
```


### 6. 잘못된 브랜치에 커밋했을 때

실수로 다른 브랜치에 커밋했을 때 해결 방법입니다.

**해결 방법**:

```bash
# 1. 현재 커밋 ID 확인
git log

# 2. 올바른 브랜치로 전환
git checkout 올바른브랜치

# 3. 해당 커밋 가져오기
git cherry-pick 커밋ID

# 4. 잘못된 브랜치로 돌아가 커밋 제거
git checkout 잘못된브랜치
git reset --hard HEAD~1  # 마지막 1개 커밋 제거
```


### 7. "Broken pipe" 오류

큰 파일을 푸시할 때 발생할 수 있는 오류입니다.

**해결 방법**:

```bash
# HTTP 버퍼 크기 증가
git config http.postBuffer 524288000

# HTTP 버전 변경
git config http.version HTTP/1.1
```


## Git 디버깅 팁

문제 해결이 어려울 때 디버깅에 도움이 되는 명령어입니다.

```bash
# SSH 연결 디버깅
GIT_SSH_COMMAND="ssh -vvv" git clone git@github.com:사용자명/저장소명.git

# HTTPS 연결 디버깅
GIT_TRACE_PACKET=1 GIT_TRACE=2 GIT_CURL_VERBOSE=1 git clone https://github.com/사용자명/저장소명.git

# Git 성능 추적
GIT_TRACE_PERFORMANCE=1 git pull
```


## 효율적인 Git 사용 팁

### 1. 정기적인 저장소 정리

```bash
# 저장소 최적화
git gc

# 불필요한 파일 정리
git prune
```


### 2. Git 별칭 사용하기

자주 사용하는 명령어를 짧게 만들어 사용할 수 있습니다.

```bash
# checkout을 co로 설정
git config --global alias.co checkout

# 상태 확인을 st로 설정
git config --global alias.st status

# 사용 예
git co develop
git st
```


