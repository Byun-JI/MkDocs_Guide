## Python설치
### python3.8.2설치
* [Windows OS(64비트) 설치](setup/Python/python3.8.2_forWindows/python-3.8.2-amd64.exe)  
>![Screenshot](img/win-py-install.png)  
>설치시, **Add Python 3.5 to PATH** 체크
<!-- * [Gzipped source tarball](setup/Python/Python-3.8.20.tgz) -->
<!-- * [XZ compressed source tarball](setup/Python/Python-3.8.20.tar.xz) -->

## 온라인 환경인 경우
### pip 최신화
pip는 파이썬 설치시, 포함되어 있습니다.  
```bash
pip install --upgrade pip
```
### MkDocs
#### MkDocs설치
```bash
pip install mkdocs
```
#### MkDocs설치 확인
```bash
mkdocs --version
```
### Material for MkDocs
```bash
pip install mkdocs-material
```

## 오프라인 환경인 경우
### 패키지 다운로드
* [MkDocs](setup/mkdocs.zip)
* [MkDocs-Material](setup/mkdocs-material.zip)
### MkDocs
*mkdocs.zip*파일의 압축을 **C:\\mkdocs**에서 해제후  
```bash
pip install --no-index --find-links="C:\\mkdocs" mkdocs
```
### Material for MkDocs
*mkdocs-material*파일의 압축을 **C:\\mkdocs-material**에서 해제후  
```bash
pip install --no-index --find-links="C:\\mkdocs-material" mkdocs-materials
```