
## GUI 도구

명령줄 인터페이스가 익숙하지 않은 개발자를 위한 Git GUI 도구들입니다.

### 1. GitKraken

GitKraken은 직관적인 시각화와 드래그 앤 드롭 기능을 제공하는 강력한 Git GUI 클라이언트입니다.

**주요 기능**:

- 시각적 브랜치 관리
- 내장된 코드 에디터
- Git-flow 지원
- 병합 충돌 해결 도구

**설치 방법**:

```bash
# 공식 웹사이트에서 다운로드
# https://www.gitkraken.com/download
```


### 2. Sourcetree

Atlassian에서 개발한 무료 Git 클라이언트로 직관적인 인터페이스를 제공합니다.

**주요 기능**:

- 시각적 히스토리 보기
- Git-flow 통합
- 원격 저장소 관리
- 병합 및 리베이스 시각화

**설치 방법**:

```bash
# 공식 웹사이트에서 다운로드
# https://www.sourcetreeapp.com/
```


### 3. VS Code Git 확장

VS Code를 사용한다면 내장된 Git 기능과 추가 확장 프로그램을 활용할 수 있습니다.

**추천 확장 프로그램**:

- GitLens: 코드 작성자, 변경 이력 등을 보여줌
- Git History: 커밋 히스토리를 시각적으로 표시
- Git Graph: 브랜치와 커밋을 그래프로 시각화

**설치 방법**:

```bash
# VS Code 확장 마켓플레이스에서 설치
```


## 생산성 향상 도구

### 1. Git Hooks

Git 작업 전후에 자동으로 스크립트를 실행하여 코드 품질을 유지할 수 있습니다.

**예시 (pre-commit hook)**:

```bash
# .git/hooks/pre-commit 파일 생성
#!/bin/sh
npm run lint  # 코드 린팅 실행
npm test      # 테스트 실행

# 실행 권한 부여
chmod +x .git/hooks/pre-commit
```


### 2. Husky

Git hooks를 쉽게 설정할 수 있는 npm 패키지입니다.

**설치 및 설정**:

```bash
# 설치
npm install husky --save-dev

# package.json 설정
{
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint && npm test",
      "pre-push": "npm run build"
    }
  }
}
```


### 3. Conventional Commits

커밋 메시지 형식을 표준화하는 도구입니다.

**설치 및 설정**:

```bash
# commitlint 설치
npm install --save-dev @commitlint/cli @commitlint/config-conventional

# 설정 파일 생성
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js

# husky와 함께 사용
npm install --save-dev husky
```

package.json에 추가:

```json
{
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  }
}
```


## 고급 Git 기능

### 1. Git Submodules

다른 Git 저장소를 하위 디렉토리로 포함할 수 있습니다.

**사용 방법**:

```bash
# 서브모듈 추가
git submodule add https://github.com/사용자명/라이브러리.git libs/라이브러리

# 서브모듈이 있는 저장소 클론
git clone --recursive https://github.com/사용자명/메인프로젝트.git

# 기존 클론에 서브모듈 초기화
git submodule init
git submodule update
```


### 2. Git LFS (Large File Storage)

대용량 파일을 효율적으로 관리할 수 있습니다.

**설치 및 설정**:

```bash
# Git LFS 설치
git lfs install

# 추적할 파일 유형 지정
git lfs track "*.psd"
git lfs track "*.zip"

# .gitattributes 파일 커밋
git add .gitattributes
git commit -m "LFS 설정 추가"
```


### 3. Git Worktree

여러 브랜치를 동시에 작업할 수 있는 기능입니다.

**사용 방법**:

```bash
# 새 작업 트리 생성
git worktree add ../프로젝트-feature feature/기능명

# 작업 트리 목록 확인
git worktree list

# 작업 트리 제거
git worktree remove ../프로젝트-feature
```


## 팀 협업 도구

### 1. GitHub Actions / GitLab CI

자동화된 테스트와 배포를 위한 CI/CD 도구입니다.

**GitHub Actions 예시** (.github/workflows/ci.yml):

```yaml
name: CI

on:
  push:
    branches: [ develop, master ]
  pull_request:
    branches: [ develop ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Node.js
      uses: actions/setup-node@v1
      with:
        node-version: '14'
    - name: Install dependencies
      run: npm ci
    - name: Run tests
      run: npm test
    - name: Build
      run: npm run build
```


### 2. Code Review 도구

코드 리뷰를 효율적으로 진행할 수 있는 도구입니다.

**GitHub Pull Requests**:

- 라인별 코멘트
- 변경 사항 비교
- 리뷰 요청 및 승인

**GitLab Merge Requests**:

- 코드 품질 분석
- CI/CD 통합
- 자동 병합 옵션


## 마무리

이 튜토리얼을 통해 Git의 기본 개념부터 Git-flow 워크플로우, 문제 해결, 팀 협업, 그리고 유용한 도구까지 살펴보았습니다. Git은 처음에는 복잡해 보일 수 있지만, 실제 프로젝트에서 꾸준히 사용하면서 익숙해지면 개발 생산성을 크게 향상시킬 수 있습니다.

신입 개발자 여러분이 이 튜토리얼을 통해 Git과 Git-flow에 대한 이해를 높이고, 팀 프로젝트에 자신감 있게 참여할 수 있기를 바랍니다. 추가 질문이나 도움이 필요하면 언제든지 선임 개발자에게 문의하세요.

행운을 빕니다! 🚀

