---
title: MyST를 쓰는 이유
authors:
  - name: Myeongseok
    affiliations: KAIST
    email: myeongseok@kaist.ac.kr
license: CC-BY-4.0
date: 2024-11-12
---

:::{figure} ../images/matheus_facure.png
:label: matheus_facure
:align: left
Matheus Facure의 저서. 이 책을 읽고 처음으로 jupyter notebook 기반 출간 방법을 알게 되었다.
:::

<br/><br/>
## Why MyST?

[인과추론 관련 저서](https://matheusfacure.github.io/python-causality-handbook/landing-page.html)를 재밌게 읽은 적이 있다. 내용도 좋았지만, 한편으로는 `ipynb` (주피터 노트북) 파일을 깃허브에 올리는 것만으로도 양질의 책을 쓸 수 있는 것이 흥미로웠다. 코드 복사, 검색 기능 지원처럼 꼭 필요한 기능에 대해서도 잘 지원했다.

해당 프로젝트 이름은 `Jupyter Book`. 다만 [개발자 블로그](https://executablebooks.org/en/latest/blog/2024-05-20-jupyter-book-myst/)를 살펴보니 지금은 Jupyter Book 2 프로젝트가 한참 진행중이었다.
> **Note** Jupyter Book 2는 아직 완성된 프로젝트가 아니다. 완성된 프로젝트가 아니다보니 관련하여 작업중인 repository도 여러 개가 있고, 내가 사용하는 `mystmd`도 그 중 하나다. 레포지토리 보기: <https://github.com/orgs/jupyter-book/repositories>

Myst 기반 여러 프로젝트를 각각 테스트해봤다. 가이드 문서가 가장 준수하다고 느껴졌고, 내 눈에 가장 예쁜 `mystmd`를 골랐다. 이미 많은 책이 출간된 `Jupyter Book`이 안전한 선택이 될 수 있지만, 여러가지 새로운 기능에 대한 기대감이 있었고, 무엇보다 버전 2(MyST 기반)를 위해 버전 1(Jupyter Book)는 유지보수 위주로 들어간다고 했기 때문.

:::{figure} ../images/export-pdf.webp
:label: export_pdf
:width: 100%
:align: center
논문 PDF export 예시이다. 마크다운으로 쓰여진 글을 템플릿을 골라 쉽게 PDF로 내보낼 수 있다. 출처: [mystmd.org](https://mystmd.org/guide/quickstart-static-exports)
:::

<br/><br/>

## MyST for Who?

1. 마크다운 기본적인 이해가 필요하다. 참고: [마크다운 튜토리얼](https://commonmark.org/help/) 
    :::{figure} ../images/markdown_tutorial.png
    :label: markdown_tutorial
    :width: 60%
    :align: left
    마크다운 튜토리얼.
    :::

2. 주피터 노트북을 사용하지 않는 사람은 굳이 사용할 필요가 있을까 싶다. 노션을 이용해서도 충분히 모든 것을 할 수 있다.

> 자신의 상황에 현재 보고 있는 출간 환경에 메리트가 있다고 느껴지면 이어지는 글을 따라가면서 자신만의 블로그나 책을 만들어보는 것도 좋겠다.