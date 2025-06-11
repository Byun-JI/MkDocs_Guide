## 정적 파일로 배포
### 방법1) 프로젝트 경로로 접근후 실행
```bash
mkdocs build
```
### 방법2) 절대 경로로 실행
```bash
mkdocs build -f C:\test_project\mkdocs.yml
```
### 배포 결과물 확인
기본 경로는 프로젝트 디렉터리 내부의 **site**디렉터리로 생성됩니다.
>`--site-dir`옵션을 추가하면 출력물 위치도 지정할수 있습니다.  
```bash
mkdocs build --site-dir ./buildTest
```

## GitHub기반 배포
### GitHub 리포지토리 생성
1. [GitHub](https://github.com) 로그인
1. [New repository](https://github.com/new) 선택
1. 아래와 같이 설정:
    * Repository name: 예) my-mkdocs-site
    * Public 또는 Private 선택 (Pages 배포 목적이면 Public 권장)
    * README 생성 여부는 선택사항 (없어도 문제 없음)
>리포지토리 생성 후 주소 예시:
>>https://github.com/your-username/my-mkdocs-site
### Git 초기화
1. Git 저장소 초기화
```bash
cd C:\test_project
mkdocs new .
git init
```
1. GitHub 리포지토리와 연결
```bash
git remote add origin https://github.com/your-username/my-mkdocs-site.git
```
### GitHub에 푸시
100MB를 초과하는 파일이 있으면 에러가 발생합니다.  
주로 첨부파일, 정적배포파일로 인하여 발생할 수 있습니다.

1. 변경된 파일들을 스테이징
```bash
git add .
```
1. 커밋(스냅샷) 생성
```bash
git commit -m "Comment입니다."
```
1. 브랜치 이름을 main으로 설정
```bash
git branch -M main
```
1. GitHub로 푸시
```bash
git push -u origin main
```
### 자동 배포 연동
GitHub Actions를 이용해 GitHub Pages에 자동 배포하는 CI/CD 설정  
아래의 설정을 마치면, 이후로는 main 브랜치가 갱신될 때 자동으로 배포가 이루어 진다.

1. GitHub Actions 권한 설정
    1. 리포지토리 → Settings > Actions > General
    1. Workflow permissions 항목에서 **Read and write permissions** 를 선택
1. GitHub Pages 설정
>GitHub Pages는 **public리포지토리**일 경우 무료로 사용가능합니다.
    1. 리포지토리 → Settings → Pages
    1. 다음과 같이 설정
        * Source: Deploy from a branch
        * Branch: gh-pages
        * Folder: / (root)
1. 프로젝트 디렉터리에 **.github**디렉터리를 생성한다.
1. .github 디렉터리에 **workflows**디렉터리를 생성한다.
1. workflows 디렉터리에 **deploy.yml**파일을 생성한다.
>디렉터리 구조 예시
```
MkDocs_Guide/
├── mkdocs.yml
├── docs/
│   └── index.md
├── .github/
│   └── workflows/
│       └── deploy.yml
```

1. **.github/workflows/deploy.yml**파일의 내용은 다음과 같이 작성한다.
```yaml
name: Deploy MkDocs to GitHub Pages

on:
  push:
    branches: [main]  # main 브랜치에 push할 때마다 실행

jobs:
  deploy:
    runs-on: ubuntu-latest  # GitHub가 제공하는 Ubuntu 런타임에서 실행

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3  # 코드 내려받기

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.8'

      - name: Install dependencies
        run: |
          pip install mkdocs mkdocs-material

      - name: Build the site
        run: mkdocs build --strict  # site/ 디렉터리 생성, --strict : 누락된 링크나 문서가 있을 경우 실패하게 함 (선택 사항)

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v4  # site/ 결과물을 gh-pages 브랜치에 자동 푸시
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}  # GitHub가 기본 제공하는 인증 토큰 (별도 설정 필요 없음)
          publish_dir: ./site  # 빌드 결과물 경로

```
1. **.github/workflows/deploy.yml** 파일을 Git 스테이지에 추가
해당 파일을 Git에 추적 대상으로 추가(staging) 합니다.
```bash
git add .github/workflows/deploy.yml
```
1. 변경사항을 커밋 (Git 히스토리에 기록)
```bash
git commit -m "Add deploy.yml"
```
1. GitHub로 푸시
로컬에서 커밋한 내용을 GitHub의 main 브랜치에 업로드
```bash
git push
```
업로드가 완료되면, GitHub는 main 브랜치에 push가 발생했음을 감지하고,  
`.github/workflows/deploy.yml` 파일에 따라 GitHub Actions가 자동 실행됩니다.
1. GitHub에서 동작 확인
    1. 리포지토리 페이지로 이동
    1. 상단 탭 중 **Actions** 클릭
    1. **“Add deploy.yml”**라는 워크플로가 실행되고 있어야 합니다.
    1. 완료 표시가 뜨면 배포 성공
1. 성공한 workflow에 들어가보면, *deploy*에 url이 표시되고 있습니다. 해당 url로 접속하면 확인가능합니다.
    * 이후로는 main브랜치에 push시 자동으로 Action이 동작하여 배포가 진행됩니다.