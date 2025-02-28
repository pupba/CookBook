## 팀 프로젝트 시나리오

실제 팀 프로젝트에서 Git-flow를 어떻게 활용하는지 단계별로 살펴보겠습니다. 웹 애플리케이션 개발 프로젝트를 예시로 사용하겠습니다.

### 프로젝트 시작 단계

1. **저장소 설정**:

```bash
# 프로젝트 디렉토리 생성 및 이동
mkdir 프로젝트명
cd 프로젝트명

# Git 초기화 및 Git-flow 설정
git init
git flow init

# 원격 저장소 연결
git remote add origin https://github.com/회사명/프로젝트명.git

# 초기 파일 생성 및 커밋
touch README.md
git add README.md
git commit -m "Initial commit"

# develop 브랜치 생성 및 푸시
git checkout -b develop
git push -u origin develop
```


### 개발 단계

2. **기능 개발 시작**:

개발자 A가 로그인 기능을 개발합니다:

```bash
git flow feature start login
# 코드 작성...
git add .
git commit -m "로그인 폼 구현"
git commit -m "로그인 API 연동"
```

개발자 B가 동시에 대시보드 기능을 개발합니다:

```bash
git flow feature start dashboard
# 코드 작성...
git add .
git commit -m "대시보드 레이아웃 구현"
```

3. **기능 개발 완료 및 코드 리뷰**:

개발자 A의 로그인 기능이 완료되었습니다:

```bash
# 원격 저장소에 feature 브랜치 푸시 (코드 리뷰용)
git push origin feature/login
```

코드 리뷰 후 기능 완료 처리:

```bash
git flow feature finish login
git push origin develop
```

4. **다른 개발자의 변경사항 가져오기**:

개발자 B는 develop 브랜치의 최신 변경사항(로그인 기능)을 가져옵니다:

```bash
# 현재 작업 임시 저장
git stash

# develop 브랜치 최신화
git checkout develop
git pull origin develop

# 다시 feature 브랜치로 돌아와 rebase
git checkout feature/dashboard
git rebase develop

# 임시 저장한 작업 복원
git stash pop
```


### 릴리스 준비 단계

5. **릴리스 브랜치 생성**:

충분한 기능이 개발되어 릴리스를 준비합니다:

```bash
git flow release start 1.0.0
```

릴리스 브랜치에서 마지막 수정 및 버전 정보 업데이트:

```bash
# 버전 정보 파일 수정
echo "version=1.0.0" > version.txt
git add version.txt
git commit -m "버전 1.0.0으로 업데이트"

# 릴리스 브랜치 푸시
git push origin release/1.0.0
```

6. **릴리스 완료**:

테스트가 완료되면 릴리스를 마무리합니다:

```bash
git flow release finish 1.0.0

# master와 develop 브랜치 푸시
git checkout master
git push origin master
git push origin --tags  # 태그 푸시

git checkout develop
git push origin develop
```


### 유지보수 단계

7. **핫픽스 적용**:

출시 후 버그가 발견되었을 때:

```bash
git flow hotfix start 1.0.1

# 버그 수정
git add .
git commit -m "로그인 실패 시 오류 메시지 수정"

# 버전 정보 업데이트
echo "version=1.0.1" > version.txt
git add version.txt
git commit -m "버전 1.0.1로 업데이트"

# 핫픽스 완료
git flow hotfix finish 1.0.1

# 변경사항 푸시
git checkout master
git push origin master
git push origin --tags

git checkout develop
git push origin develop
```


## 팀 협업 모범 사례

### 1. 커밋 메시지 규칙

일관된 커밋 메시지 형식을 사용하면 변경 이력을 이해하기 쉽습니다:

```
feat: 새로운 기능 추가
fix: 버그 수정
docs: 문서 변경
style: 코드 포맷팅, 세미콜론 누락 등
refactor: 코드 리팩토링
test: 테스트 코드 추가/수정
chore: 빌드 프로세스 변경, 패키지 매니저 설정 등
```

예시:

```bash
git commit -m "feat: 사용자 프로필 이미지 업로드 기능 추가"
git commit -m "fix: 모바일에서 로그인 버튼이 보이지 않는 문제 해결"
```


### 2. Pull Request 활용

코드 리뷰와 품질 관리를 위해 Pull Request를 활용하세요:

1. feature 브랜치를 원격 저장소에 푸시
2. GitHub/GitLab에서 develop 브랜치로의 PR 생성
3. 코드 리뷰 및 CI/CD 테스트 진행
4. 승인 후 병합

### 3. 정기적인 동기화

다른 개발자의 변경사항과 충돌을 방지하기 위해 정기적으로 develop 브랜치와 동기화하세요:

```bash
git checkout develop
git pull origin develop
git checkout feature/내기능
git rebase develop
```

다음 챕터에서는 Git과 관련된 유용한 도구와 확장 기능을 소개하겠습니다.

