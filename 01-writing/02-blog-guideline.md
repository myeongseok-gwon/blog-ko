---
title: 블로그 가이드라인
authors:
  - name: Myeongseok
    affiliations: KAIST
    email: myeongseok@kaist.ac.kr
license: CC-BY-4.0
date: 2024-11-13
---

이 글은 [MyST 공식 문서 가이드라인](https://mystmd.org/guide)을 참고하고, 중요한 부분을 간추려 요약한 글이다. 

<br/><br/>

## 0. Prerequisites 

1. 커맨드에 `python3 --version`를 입력하여 Python3 설치를 확인한다.
2. 커맨드에 `git --version`를 입력하여 Git 설치를 확인한다.
3. tex 관련 패키지를 설치하자. (https://www.tug.org/texlive/) Mac에서는 MacTeX를 설치하면 된다. (논문 포맷의 pdf 파일을 만들 때 필요하다.)

<br/><br/>

## 1. Install & Initialize
작업할 디렉토리에서 다음 커맨드를 실행하여 [Python 가상환경](https://docs.python.org/ko/3/library/venv.html)을 구성하고 패키지를 설치하자. 참고로 터미널을 새로 열면 아래 커맨드 중 `source ./venv/bin/activate`를 실행해야 가상환경을 사용할 수 있다. 패키지는 한번만 설치하면 된다. 그런다음 `myst init`를 입력하면 초기 셋팅은 끝난다. `myst start`를 바로 실행할 것인지 물을텐데, 당장 시작할 필요는 없으므로 N을 입력하자.  

```bash
python3 -m venv venv
source ./venv/bin/activate
pip install mystmd
myst init
```


<br/><br/>

## 2. Directory Structure
mystmd는 자동으로 md 및 ipynb 파일을 읽기 때문에 특별한 설정이 필요 없다. 폴더 내부의 파일까지도 자동으로 읽는다. 추가적으로 폴더명 및 파일명에 `01-`, `02-` 형태로 인덱싱하면 그 순서를 이해한다는 점을 알아두자. 또 같은 폴더 내에 폴더와 파일이 있으면 파일을 먼저 읽고, 폴더 내부의 파일을 읽는다. 그래서 `about-me.md` 파일을 root 폴더에 두고, 나머지 자료는 폴더 내부에 관리하게 되면 about-me 파일이 최상단에 위치하게 된다.

이 블로그의 디렉토리 구조는 다음과 같다. (README.md는 블로그의 메인 페이지이다.)

```bash
README.md
about-me.md
01-writing/
├── 01-why-i-use-myst.md
├── 02-myst-tutorial.md
02-research/
├── 01-research-1.md
├── 02-research-2.md
```

<br/><br/>
## 3. myst.yml
`myst init`을 실행하면 자동으로 생성되는 가장 중요한 파일, myst.yml은 블로그의 모든 설정을 담고 있다. 아래 첫 줄 주석에 적혀있듯이 자세한 정보는 [공식 문서](https://mystmd.org/guide/frontmatter)를 참고하자. 몇 가지 옵션을 소개하자면 다음과 같다.
- exclude 부분에 적혀있는 폴더들은 블로그에 포함되지 않는다. 
- github 부분에 자신의 github 주소를 적어두면 블로그 상단에 github 아이콘이 생성된다.
- site에서는 블로그의 테마, favicon, logo 등을 설정할 수 있다.

```yaml
# See docs at: https://mystmd.org/guide/frontmatter
version: 1
project:
  id: 7f4425d6-d197-4904-88a4-81cd23fa671d
  # title:
  # description:
  # keywords: []
  # authors: []
  github: https://github.com/myeongseok-kwon/blog
  # To autogenerate a Table of Contents, run "myst init --write-toc"
  exclude:
    - venv/**
    - draft/**
    - data/**
    - images/**
site:
  template: book-theme
  options:
    favicon: images/favicon.ico
    logo: images/site_logo.png
```

<br/><br/>
## 4. myst start
`myst start`를 입력하면 블로그를 실행할 수 있다. 블로그는 기본적으로 로컬에서 실행되므로 인터넷에 연결되어 있어야 한다. 블로그를 종료하려면 `Ctrl + C`를 누르면 된다. 실행시킨 터미널에서 주소를 확인할 수 있는데, 기본적으로는 https://localhost:3000에서 실행된다. 마크다운, ipynb 파일을 추가하고 수정하면서 저장하면 실시간으로 블로그가 변하는 것을 확인할 수 있다.


<br/><br/>
## 5. Deployment
[github pages 배포 가이드라인](https://mystmd.org/guide/deployment-github-pages)를 참고하여 블로그를 배포하자. 요약하면 다음과 같다.
1. github 레포지토리를 만들고 (public), 작업한 디렉토리를 push한다.
2. github 레포지토리에서 Settings -> Pages -> Source에서 Github Actions 배포 방법을 선택한다.
3. myst init --gh-pages