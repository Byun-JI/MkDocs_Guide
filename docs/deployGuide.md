## 정적 파일로 배포
### 방법1) 프로젝트 경로로 접근후 실행
`mkdocs build`
### 방법2) 절대 경로로 실행
`mkdocs build -f C:\test_project\mkdocs.yml`
### 배포 결과물 확인
기본 경로는 프로젝트 디렉터리 내부의 **site**디렉터리로 생성됩니다.  
`--site-dir`옵션을 추가하면 출력물 위치도 지정할수 있습니다.  
예시 : `mkdocs build --site-dir ./buildTest`

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
### 로컬 MkDocs 프로젝트의 Git 초기화
```bash
cd C:\test_project
mkdocs new .
git init
git remote add origin https://github.com/your-username/my-mkdocs-site.git
```
### GitHub에 푸시
```bash
git add .
git commit -m "Initial MkDocs setup"
git branch -M main
git push -u origin main
```