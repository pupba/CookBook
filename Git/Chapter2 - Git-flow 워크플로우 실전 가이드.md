## Git-flow 시작하기

Git-flow를 시작하려면 먼저 초기화가 필요합니다. 기존 Git 저장소에서 다음 명령어를 실행합니다:

```bash
git flow init
```

이 명령어는 브랜치 구조를 설정하는 과정을 안내합니다. 대부분의 경우 기본값을 사용해도 좋습니다.

## 기능 개발하기 (Feature)

새로운 기능을 개발할 때는 feature 브랜치를 사용합니다.

### 기능 브랜치 시작하기:

```bash
git flow feature start 기능이름
```

이 명령어는 `develop` 브랜치에서 분기된 `feature/기능이름` 브랜치를 생성합니다.

### 예시:

로그인 기능을 개발한다면:

```bash
git flow feature start login
```

이제 `feature/login` 브랜치에서 작업을 시작할 수 있습니다.

### 기능 개발 완료하기:

기능 개발이 완료되면 다음 명령어로 마무리합니다:

```bash
git flow feature finish 기능이름
```

이 명령어는:

1. `feature/기능이름` 브랜치를 `develop` 브랜치에 병합합니다.
2. 기능 브랜치를 삭제합니다.
3. `develop` 브랜치로 전환합니다.

## 릴리스 준비하기 (Release)

개발이 완료되어 출시 준비가 되면 release 브랜치를 사용합니다.

### 릴리스 브랜치 시작하기:

```bash
git flow release start 버전번호
```

예: `git flow release start 1.0.0`

이 브랜치에서는 버그 수정, 문서 작성 등 릴리스 준비 작업만 수행합니다.

### 릴리스 완료하기:

```bash
git flow release finish 버전번호
```

이 명령어는:

1. 릴리스 브랜치를 `master`와 `develop` 브랜치에 병합합니다.
2. `master`에 버전 태그를 생성합니다.
3. 릴리스 브랜치를 삭제합니다.

## 핫픽스 처리하기 (Hotfix)

출시된 버전에서 긴급 버그가 발견되면 hotfix 브랜치를 사용합니다.

### 핫픽스 브랜치 시작하기:

```bash
git flow hotfix start 버전번호
```

예: `git flow hotfix start 1.0.1`

### 핫픽스 완료하기:

```bash
git flow hotfix finish 버전번호
```

이 명령어는:

1. 핫픽스 브랜치를 `master`와 `develop` 브랜치에 병합합니다.
2. `master`에 새 버전 태그를 생성합니다.
3. 핫픽스 브랜치를 삭제합니다.

## 실제 워크플로우 예시

신입 개발자가 새 기능을 개발하는 과정을 살펴보겠습니다:

1. 최신 develop 브랜치 가져오기:

```bash
git checkout develop
git pull origin develop
```

2. 새 기능 브랜치 시작:

```bash
git flow feature start user-profile
```

3. 코드 작성 및 커밋:

```bash
# 파일 수정 후
git add .
git commit -m "사용자 프로필 페이지 레이아웃 구현"
```

4. 기능 완료 및 병합:

```bash
git flow feature finish user-profile
```

5. 변경사항 원격 저장소에 푸시:

```bash
git push origin develop
```


다음 챕터에서는 Git 사용 시 자주 발생하는 문제와 해결 방법에 대해 알아보겠습니다.

