---
title: R 크롤링 R Language / R Studio 설치
author: Lee
date: 2020. 2. 9. 13:02 +0900
categories: [R, Crawling]
tags: [r, R langage, R Studio 설치, R 설치, RStudio]
math: true
mermaid: true
image:
  src: 
---

이 페이지에서는 R language & RStudio 설치방법만 알아보겠습니다

 

우선 R을 설치하기 전 권장사항이 있습니다(거의 필수)

사용하시는 PC / User / 작업 폴더 이름을 영어로만 지정해주세요

 

R을 포함한 다른 도구를 사용할 때도 권장합니다 

어떤식으로든 문제가 일어나는 부분이고 실제로 꽤 많은 오류 해결법이기도 합니다

( Code자체는 한글을 사용해도 무방합니다. 개인적으로 가독성을 위해 한글을 많이 사용하려 합니다)

 

 

그럼 R language를 설치하겠습니다

https://www.r-project.org/로 이동합니다

그리고 download R을 클릭합니다


 
 

 

 

국가별 링크가 쭉 나옵니다

어떠한 링크를 선택해도 상관없지만 가장 위 https://cloud.r-project.org/를 선택합니다


 

링크를 선택합니다

 

운영체제에 맞는 다운로드 링크를 선택합니다

제 PC는 Windows 체제이니까 Download R for Windows를 선택하겠습니다


 

 

우리는 R을 처음으로 설치하기에 install R forthefirst time.를 선택합니다

솔직히 말해서 다른링크는 누른적이 없습니다....


 

 

20년 2월 기준 가장 최신 R 3.6.3버전을 설치합니다

경험상 특별히 성능이 좋다거나 범용성이 뛰어난 버전은 없었습니다

가장 최신 버전을 설치하시면 됩니다


 

 

다운 받은 파일 압축을 풀면 아래와 같은 팝업이 뜹니다


 

 

 

아래와 같은 팝업이 나올때까지 다음을 눌려주시면 됩니다

core Files / 32-bit / 64-bit / message translations 모두 설치해주시면 됩니다


 

 

아이콘 생생은 편하실대로 하시면 됩니다

레지스트리 항목은 전부 체크해주시는 편합니다 이후에는 다음(N)을 누르시고 설치


 

 

R을 실행하시면 이런 창이 뜹니다

처음보시면 느낌이 굉장히 심심합니다


 

 

 

 

보다 편리한 사용을 위해 Rstudio를 설치합니다 

구글에 Rstudio를 검색하여 https://rstudio.com/products/rstudio/download/로 접속합니다


 

Desktop 버전을 다운로드 합니다


 

 

 

해당하는 운영체제에 맞는 Rstudio 다운로드 링크를 찾아갑시다

저는 윈도우 체제를 찾아갑니다


 

 

 

다운 받은 파일을 실행 시킨후 

계속 다음(N)을 눌러주시면 별 무리 없이 RStudio 설치가 가능합니다 


 

 

 

RStudio를 실행해봅시다

주의할 점이 있는데 아이콘을 우클릭하여 관리자 권한으로 실행(A)합니다


 

 

 

RStudio의 모습입니다

순정(?) R language 보다는 사용자 편의를 위한 옵션과 UI가 잘 갖추어져 있습니다

커스터 마이징을 하고싶으시다면 

Tools > Global Options에 들어가면 여러 옵션 적용이 가능합니다

그 중에서 필수 옵션 하나만 해봅시다


 

 

 

Global Options에 들어가서

Code를 선택하고 Soft-wrap R source files를 체크합니다

간단히 말해 자동 줄바꿈기능인데

RStudio창 크기가 바뀔때마다

code을 줄을 자동적으로 바꿔주는 기능입니다

code가 길어지는 웹 크롤링에서는 필수적이라 할 수 있는 기능입니다


 

 

 

이처럼 무지막지하게 길어지는 code의 가독성을 높여줍니다

코드가 길어져도 문제없죠


 

 

그럼 다음글에서 

rjava와 RSelenium과 설치방법을 알아보겠습니다
