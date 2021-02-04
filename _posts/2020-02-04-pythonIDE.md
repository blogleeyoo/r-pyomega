---
title: 개발 환경 구축
author: Yoo
date: 2020-02-04 00:33:00 +0900
categories: [python, 개발 준비]
tags: [Anaconda, VS Code, Jupyter, python IDE]
math: true
mermaid: true
image:
  src: 
---

## 리스트

- [x] 개발 환경 구축
  - [x] 개발 환경 선택하기
  - [x] Anaconda 설치해보기
- [ ] Cloud Computing Service에 분석용 python 환경 구축하기
  - [ ] Cloud Computing 선택하기
  - [ ] python 구축하기

# 개발 환경 선택하기

  - 개발 환경은 철저히 개인 취향이지만 업무에 따라 조금 달라지는 감이 있습니다.
  - 제 추천은 Pycharm | Sublime | VsCode | Jupyter 입니다.
  - 개발과 연계되는 python은 Pycharm Professional | VS Code 중 자유롭게 선택해주세요.
    
    1. Pycharm은 IDE입니다. 다양한 기능을 가지고 있고 python과 그 패키지들의 고유의 기능을 강화하거나 보조해줍니다.
      다양한 운영체제에서 자기 기능을 잘 해줍니다. windows가 아니라면 최상의 선택일 것입니다.
      하지만 professional이 아니라면 main IDE로 고려하지 않으셔도 좋습니다.

    2. VS Code는 IDE는 아니지만 다양한 라이브러리를 통해서 기능을 강화할 수 있습니다. 다른 언어 개발도 곧잘 할 수 있으므로 다양한 언어를 사용한다면 추천합니다. 저는 별의별 짓을 다해서 VS Code를 씁니다.
      windows 사용자라면 최상의 선택이라고 생각합니다. 

  - Data 계통의 python user라면 jupyter를 활용하시길 추천합니다.
    저는 VS Code와 jupyter를 같이 사용합니다. 

  - 다른 목적의 python user라면 Sublime text를 추천합니다. 저는 주력으로 쓰지 않지만 매력이 터지는 프로그램입니다. 

[*참조 : Top 10 Python IDE and Code Editors in 2020*](https://www.geeksforgeeks.org/top-10-python-ide-and-code-editors-in-2020/)

# Anaconda 설치해보기

Anaconda는 저와 같은 개발 환경을 선택한 사람에게 있어서 가장 간편한 선택사항입니다. 그러나 더 큰 파이썬 사용자가 되는 길에 있어서 장애물이기도 합니다. 계속 귀찮게 하는 녀석이죠.

pip가 아닌 conda로 패키지와 가상환경을 관리하는 것이 언젠가는 불편을 유발합니다.

그러나 아나콘다는 데이터 사이언스를 위한 기초적인 패키지가 포함된 설치를 제공하므로 이용해봅시다.

[Anaconda 설치 링크](https://www.anaconda.com/products/individual)

![다운로드](/assets/img/items/anaconda-download.png)

![다운로드](/assets/img/items/anaconda-installer.png)

새해 기념인지 홈페이지가 리뉴얼되었는데 개인적으로 별로내요. 비즈니스 모델에 변화가 있었던 모양입니다.

설치 과정은 따로 언급하지 않겠습니다. 

![다운로드](/assets/img/items/navigator1.png)

제가 원하는 프로그램이 모두 설치되어 있네요. 굳

![다운로드](/assets/img/items/navigator2.png)

제가 원하는 패키지도 대부분 설치되어 있습니다. 