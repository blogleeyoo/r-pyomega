---
title: R 크롤링 디시인사이드 (DC Inside) GET / POST + Selenium (셀레니움)
author: Lee
date: 2020-2-10 10:49 +0900
categories: [R, Crawling]
tags: [GET crawling, POST crawling, r, r crawling, R language, R 크롤링, Selenium, 디시인사이드, 디시인사이드 크롤링, 셀레니움]
math: true
mermaid: true
image:
  src: 
---
<br>
크롤링에 필요한 패키지(package)와 라이브러리(library)는 아래와 같습니다<br>
<br>
```R
install.packages(c("dplyr", "httr", "jsonlite", "rJava", "RSelenium", "stringr")<br>

· library(dplyr)<br>
· library(httr)<br>
· library(jsonlite)<br>
· library(rJava)<br>
· library(RSelenium)<br>
· library(stringr)<br>
```
 <br>
<br>
 <br>
<br>
이외에도 더 필요한 패키지 및 라이브러리는 그때 그때 언급하겠습니다. <br>
사실 R로 다양한 작업을 하다보면 필요한 라이브러리는 구분하지 않고 Rstudio를 실행할때 한꺼번에 불어오는 편입니다.<br>
(시간도 많이 걸리지 않아 굳이 구분하는 것보다는 편리하기 때문이죠. 저 같은 경우는 50개 가까운 라이브러리를 불러옵니다..)<br>
 <br>
<br>
그럼 간단하게 디시인사이드 갤러리를 입문 겸 크롤링 해보겠습니다<br>
<br>
중간중간 설명을 붙이겠지만 이해가 잘 안되시는 유저들은 스크린샷과 명령어에 따라 실습하는걸 추천합니다<br>
<br>
 <br>
<br>
크롤링 대상은 제가 요즘 빠져있는 여배우 "아라가키 유이"의 갤러리 입니다<br>
<br>
해당 사이트는 구조가 복잡하지 않고 게시글 숫자가 많지 않아 연습으로 제격입니다 (사심이 많이 들어갔네요 하하)<br>
![gaki1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbM0PlP%2FbtqBO8oWVaY%2Fsn5wfw7t06z6GzeVzI0fEk%2Fimg.gif)
<br>
<br>
유난히 사진이 빛이 나보이는건 기분 탓이겠죠?<br>
 <br>
<br>
 <br>
<br>
그럼 "아라가키 유이 갤러리"로 가보겠습니다<br>
<br>
https://gall.dcinside.com/mgallery/board/lists?id=aragakiyui<br>
<br>
 <br>
<br>
<br>
 <br>
<br>
URL주소를 살펴볼 필요가 있습니다<br>
<br>
https://gall.dcinside.com/mgallery/board/lists?id=aragakiyui를 잘 살펴봅시다.<br>
<br>
잘은 모르겠지만 "?"를 기점으로 분리해봅시다<br>
<br>
 <br>
<br>
 <br><br>
<br>
URL : https://gall.dcinside.com/mgallery/board/lists<br>
<br>
Query String(추가적 정보) :<br><br>
<br>
1) id=aragakiyui<br>
<br>
 <br>
<br>
우리가 하루에도 수십번씩 접속하는 흔히 말하는 URL은 사실은 URL + Query String로 나뉘어져 있는 URI(Uniform Resourse Indicator)입니다.<br>
URL은 URI은 부분집합이라 할수 있죠. 용어는 잊어버리셔도 됩니다.<br>
<br>
우리가 기억할 것은 두 부분집합의 요소는 "?"로 결합되어 있다는 것입니다<br>
<br>
 <br>
<br>
 <br>
<br>
그럼 두번째 페이지로 가봅시다<br>
<br>
https://gall.dcinside.com/mgallery/board/lists/?id=aragakiyui&page=2<br>
<br>
첫번째 페이지에서 볼수 없었던 요소가 추가되었습니다<br>
<br>
 <br>
<br>
 <br>
<br>
URL : https://gall.dcinside.com/mgallery/board/lists<br>
<br>
Query String(추가적 정보) :<br>
<br>
1) id=aragakiyui<br>
<br>
2) &page=2<br>
<br>
 <br>
<br>
 <br>
<br>
느낌이 오시나요?<br>
<br>
2)의 page주소를 바꾸어 봅시다<br>
<br>
https://gall.dcinside.com/mgallery/board/lists/?id=aragakiyui&page=1 <- 첫번째 페이지<br>
<br>
https://gall.dcinside.com/mgallery/board/lists/?id=aragakiyui&page=200 <- 마지막 페이지<br>
<br>
각 링크를 타고가면 첫번째 페이지와 200번째 페이지로 바로갈수 있습니다.<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
우리가 처음 접속했던<br>
<br>
https://gall.dcinside.com/mgallery/board/lists?id=aragakiyui와<br>
<br>
https://gall.dcinside.com/mgallery/board/lists/?id=aragakiyui&page=1는 사실 동일한 페이지입니다<br>
<br>
우리 눈에는 다르게 보였을 뿐이죠 <br>
<br>
URL구조가 이제 어떠한 알고리즘(?)인지 감이 오실겁니다<br>
<br>
 <br>
<br>
 <br>
<br>
그럼 크롤링을 위한 작업을 시작합시다 <br>
<br>
두번째 페이지로 이동합시다<br>
<br>
그리고 F12를 누릅니다<br>
<br>
 <br>
<br>
 <br><br>
<br>
다음과 같은 난생 처음 볼법한 페이지가 뜹니다(저는 검정바탕화면인데 대부분 이용자는 하양바탕이 보통입니다)
![gaki2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FU7US8%2FbtqBPW2EHFm%2FwqL48aaA1JXX48GaFwFjd1%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
Network를 눌러봅시다<br>
![gaki3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcFLKPg%2FbtqBQodxOC3%2FDiJFUV896Y0tnegxLm0dIK%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
그리고 새로고침(F5)을 눌러봅시다<br>
<br>
뭔지 모를 것들이 차곡차곡 쌓입니다 ㅎㅎ<br>
<br>
우리가 보는 페이지는 이렇게 많은 요소로 구성되어 있습니다<br>
<br>
크롤링에 필요한 정보를 찾아봅시다<br>
<br>
디시인사이드는 입문자용잡게 우리에게 필요한 정보는 가장 위에 있습니다<br>
<br>
클릭합니다<br>
![gaki4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FefiISq%2FbtqBO7QRFGD%2FgkD7kORkpx7Asa63Gufbo1%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
Header - General에서 필요한 정보는 Request URL과 Request Method입니다<br>
![gaki5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbcxkVb%2FbtqBO8hXmX2%2FiqlVySV5y4j4wLVm7YyG4K%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
아래로 내려가 Query String Parameter로 가서 정보를 확인합니다<br>
![gaki6](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2jXwi%2FbtqBQ2uwAvw%2FerqpGb4WZQaK69QARbZoHK%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
종합해보자면<br>
<br>
디시인사이드 아라가키 유이 갤러리는 GET방식의 URL과  id / page로 이루어져 있습니다<br>
<br>
이러면 우리가 필요한 정보를 다 얻었습니다<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
여기까지 해당 R 명령어는 다음과 같습니다<br>
<br>
 <br>
<br>
### HTTP 요청 및 응답<br>
```R
res_yui <- GET(url = 'https://gall.dcinside.com/mgallery/board/lists/', 
                     query = list(id = 'aragakiyui', 
                     page=1))
```
<br>
 <br>
<br>
우선 첫번째 페이지를 시험삼아 크롤링 해봅시다<br>
<br>
크롤링의 큰 그림은 이렇습니다<br>
<br>
```
1) 해당 게시글을 링크를 수집한다 (중요)

2) 수집한 링크에서 데이터(요소 : Element)를 찾아서 수집한다
```
<br>
 <br>
<br>
크롤링에서 가장 우선적으로 해야하는 것은 "링크" 수집입니다<br>
<br>
링크 수집을 완전하게 한다면 크롤링의 절반이상은 끝난것이나 다름없습니다<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
그러면 게시글 링크을 긁어봅시다<br>
<br>
Elements를 클릭하고 제일 왼쪽 위 아이콘(Ctrl + Shift + C)을 클릭합시다<br>
![gaki7](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcbq2p2%2FbtqBQnTctFH%2FgVJToAKulVCGnhC9ybxY21%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
글 제목에 마우스를 올리면 이런 팝업창이 뜹니다<br>
<br>
제목을 클릭해봅시다<br>
![gaki8](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeVcRUZ%2FbtqBSPuHqP1%2FCYSSnaScsyOzppgzkKFwW0%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
클릭한 제목에 해당하는 Element가 나옵니다<br>
<br>
링크 뿐만 아니라 게시글 제목 또한 나옵니다<br>
![gaki9](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsBkSq%2FbtqBSPnUXzA%2FRbL2KPEtTIneWSELD4NbN1%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
스크린샷으로 확인한 정보를 바탕으로 작성한 링크를 크롤링하는 R명령어 입니다<br>
<br>
  <br>
<br>
<br>
#### 각키갤 링크 수집
```
링크_각키 <- c()                                    ## "링크_각키" 라는 빈 벡터(vector)공간을 만듭니다
```
```R
링크.각키.tmp <- res_yui %>%
  read_html() %>% 
  html_nodes('td.gall_tit.ub-word') %>% ##링크 주소 바로 위에 있는 Element입니다 
  html_nodes('a:nth-child(1)') %>%  ## 게시글 링크 주소는td.gall_tit.ub-word 밑  첫번째 "a"에 있습니다 
  html_attr('href') %>%                              ## html_attr()은 해당 속성 값을 긁어옵니다
  unique()                                                   ##중복되는 링크를 제거해주는 함수입니다

if (length(링크.각키.tmp) == 0) {
  링크_각키 <- append(링크_각키, "수동확인")
} else {
  링크_각키 <- append(링크_각키, 링크.각키.tmp)
}  ## 수집한 링크가 없을경우 "수동확인", 제대로 수집한 경우는 "링크_각키" 벡터 공간에 저장합니다 
```
<br>
 <br>
<br>
 <br>
<br>
```
※ element에 가장 앞에있는 글자(td)와 class(gall_tit.ub-word)를 점(.)으로 이어줍니다

※ 참고로 class없이 id만 있는 경우는 "#"으로 이어줍니다 예시: td#gall_tit.ub-word

※ 해당 element에 html_nodes("element")를 입력했음에도 정보를 크롤링하지 못한경우에는 상위 단계의 element를 추가합니다 (예시를 보겠습니다)
```
<br>
 <br>
<br>
 <br>
<br>
    <br>             
<br>
```
해당 단계 element
```
```R
링크.각키.tmp <- res_yui %>% 
  read_html() %>%  
  html_nodes('td.gall_tit.ub-word') %>%            
  html_nodes('a:nth-child(1)') %>%                 
  html_attr('href') %>%                                  
  unique()               
```
```
상위 단계 element 추가
```
```R
링크.각키.tmp <- res_yui %>% 
  read_html() %>%  

  html_nodes('tr.ub-content us-post') %>% 


  html_nodes('td.gall_tit.ub-word') %>%            
  html_nodes('a:nth-child(1)') %>%                 
  html_attr('href') %>%                                  
  unique()                    
```
  <br>
  <br>
  <br>
양쪽 결과는 같습니다<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
제대로 구동되었는지 확인합시다<br>
<br>
한 페이지당 50개 게시글이 있는데 각각의 링크가 제대로 저장되었습니다<br>
![gaki10](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcjX9r1%2FbtqBS6XivbF%2Fb7hgdiW7Qrer0xeGDIDvPK%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
링크_각키[1] 를 실행하면 최상위 게시글의 링크를 확인할 수 있습니다<br>
<br>
그런데 ""/mgallery/board/view/?id=aragakiyui&no=6037&page=1" 이 링크로 접속하면 해당 게시글로 갈 수 없습니다<br>
<br>
"https://gall.dcinside.com"가 빠졌기 때문입니다<br>
<br>
 <br>
<br>
 <br>
<br>
아래 R명령어를 추가적으로 실행합니다<br>
<br>
```R
링크_각키 <- paste0("https://gall.dcinside.com/",링크_각키)<br>
```
<br>
<br>
 <br>
<br>
 <br>
<br>
각키갤 첫번째 게시글 링크를 확인 합시다<br>
<br>
링크_각키[1]<br>
<br>
> https://gall.dcinside.com//mgallery/board/view/?id=aragakiyui&no=6037&page=1
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
이제 우리는 수집한 링크를 토대로 게시글의 Element를 수집하겠습니다<br>
<br>
개인적으로 링크 수집 후 element 수집은 셀레니움을 선호합니다<br>
<br>
(다음 게시글에 only GET/POST 방식을 포스팅하겠습니다)<br>
<br>
 <br>
<br>
 <br>
<br>
selenium(셀레니움)을 켭니다<br>
<br>
```
cmd를 켠 상태에서<br>
<br>
1) cd C:\r_selenium<br>
<br>
2) java -Dwebdriver.gecko.driver="geckodriver.exe" -jar selenium-server-standalone-4.0.0-alpha-1.jar -port 4445<br>
<br>
입력후 엔터로 실행합니다<br>
```
<br>
 <br>
<br>
그리고 R에서 아래 명령어를 실행합니다<br>
<br>
 <br>
<br>
 <br>
<br>
## RSelenium<br>
```R
remDr <- remoteDriver(remoteServerAddr="localhost",  
                      port=4445L,  
                      browserName="chrome") 
remDr$open()
```
<br>
 <br>
<br>
작업표시줄에 크롬페이지가 하나 구동됩니다<br>
<br>
요놈이 셀레니움으로 자동화할 수 있는 페이지 입니다<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
## 수집하려는 요소의 벡터공간을 사전에 만듭니다<br>
<br>
```R
제목_각키 <- c()

작성자_각키 <- c()

날짜_각키 <- c()

본문_각키 <-c()

주소_각키 <-c()           ## element를 수집한 링크 주소를 하나씩 저장합시다
```
<br>
 <br>
<br>
 <br>
<br>
## 각키갤 첫번째 게시글로 이동합니다<br>
<br>
```R
remDr$navigate(링크_각키[1])
```
<br>
<br>
 <br>
<br>
## 게시글의  html요소를 읽어옵니다<br>
```R
body <- remDr$getPageSource()[[1]]

body <- body %>% read_html()
```
 <br>
<br>
 <br>
<br>
## 제목 수집<br>
<br>
F12를 누르고 클릭하여 Element의 위치를 파악합시다<br>
<br>
아름다운 그녀의 얼굴이 짤리지 않게 스크린샷을 찍었습니다<br>
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
제목을 수집하는 명령어는 위 스크린샷을 기반으로 작성하면 다음과 같습니다<br>
```R
제목.각키.tmp <- body %>%  
  html_nodes("span.title_subject") %>%  
  html_text() 


if (length(제목.각키.tmp) == 0) { 
  제목_각키 <- append(제목_각키, "수동확인") 
} else { 
  제목_각키 <- append(제목_각키, 제목.각키.tmp) 
}  ## 수집한 제목 없을 경우 "수동확인", 제대로 수집한 경우는 "제목_각키"벡터 공간에 저장합니다
```

이와 동일하게 작성자 / 날짜 / 본문을 수집하겠습니다<br>
<br>
<br>
<br>
 <br><br>
<br>
 <br>
<br>
##작성자 수집<br>
```R
작성자.각키.tmp <- body %>%  
  html_nodes("div.fl") %>%  
  html_nodes("span.nickname") %>%  
  html_nodes("em") %>% 
  html_text() 


if (length(제목.각키.tmp) == 0) { 
  작성자_각키 <- append(작성자_각키, "수동확인") 
} else { 
  작성자_각키 <- append(작성자_각키, 작성자.각키.tmp) 
}  ## 수집한 작성자 없을 경우 "수동확인", 제대로 수집한 경우는 "작성자_각키"벡터 공간에 저장합니다 
```


 

##날짜 수집<br>
```R
날짜.각키.tmp <- body %>%  
  html_nodes("span.gall_date") %>%  
  html_text() 


if (length(날짜.각키.tmp) == 0) { 
  날짜_각키 <- append(날짜_각키, "수동확인") 
} else { 
  날짜_각키 <- append(날짜_각키, 날짜.각키.tmp) 
}  ## 수집한 작성자 없을 경우 "수동확인", 제대로 수집한 경우는 "작성자_각키"벡터 공간에 저장합니다 
```
 <br>
<br>
 <br>
<br>
## 본문 수집<br>
```R
본문.각키.tmp <- body %>%  
  html_nodes("div.writing_view_box") %>%  
  html_text() 


if (length(날짜.각키.tmp) == 0) { 
  본문_각키 <- append(본문_각키, "수동확인") 
} else { 
  본문_각키 <- append(본문_각키, 본문.각키.tmp) 
}  ## 수집한 본문 없을 경우 "수동확인", 제대로 수집한 경우는 "본문_각키"벡터 공간에 저장합니다 
```
<br>
<br>
 <br>
<br>
 <br>
<br>
## 주소(URL)<br>
```R
주소_각키 <- append(주소_각키, 링크_각키[1])     # 나중에 for문을 이용할때는 [1]을 바꾸어줍니다 지금은 편의상 [1]로
```
 <br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
## 데이터 전처리<br>
<br>
본문 결과를 확인해봅시다<br>
![gaki11](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbDB14i%2FbtqBO8CsLmV%2F86zRjlu6kbp5qzavKtTyDk%2Fimg.png)
<br>
 <br>
<br>
의미를 알수 없는 역슬래쉬를 포함한 글자들이 있습니다<br>
<br>
이를 정규표현식이라고 합니다<br>
<br>
정규표현식은 특정한 규칙을 가진 문자열의 집합을 표현하는 데 사용하는 형식 언어입니다<br>
<br>
우리가 웹상으로 보는 텍스트는 눈에 보이지 않지만 정규표현식을 포함하는 경우가 대부분입니다<br>
<br>
R 뿐만이 아니라 대부분 프로그래밍 언어는 정규표현식을 지원합니다<br>
<br>
자세한 설명은 다음기회에 하고 우리는 이를 지우는 전처리를 하겠습니다<br>
<br>
삭제 명령어는 str_replace_all( )를 이용합니다<br>
<br>
 <br>
```R
본문_각키 <- str_replace_all(본문_각키,"\n","") 
본문_각키 <- str_replace_all(본문_각키,"\t","")
```
![gaki12](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcfhCd4%2FbtqBSOv3TwR%2Fzh1ZGrvtLF6CxPVzyOQ0t0%2Fimg.png)

 <br>
<br>
<br>
 <br>
<br>
쉽게 전처리를 완료하였습니다<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
## 데이터프레임으로 합치기 및 csv파일로 저장<br>
<br>
 <br>
<br>
그럼 데이터 프레임으로 크롤링한 elements을 하나의 파일로 모아서 csv파일로 저장합니다<br>
```R
df_각키 <- data.frame(제목_각키, 작성자_각키, 날짜_각키, 주소_각키) 
write.csv(df_각키, file = "D:/df_각키.csv", row.names=FALSE)
```
<br>
D드라이브로 가면 결과를 확인할 수 있습니다<br>
<br>
csv파일로 문제없이 저장되었습니다<br>
![gaki13](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fby8lX1%2FbtqBO8oYSjv%2FNLQAnKXVUo3TeGxzYf6wr0%2Fimg.png)

<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
## for문으로 자동화하기(중요!!)<br>
<br>
첫 페이지 크롤링을 모두 완료하였습니다<br>
<br>
이를 바탕으로 자동화 시스템(거창하네요 ㅎㅎ;;)를 구축해야합니다<br>
<br>
여기까지 오셨다면 어렵지 않게 구축할 수 있습니다<br>
<br>
다양한 방법이 있지만 for문과 tryCatch( )를 가장 많이 활용합니다<br>
<br>
 <br>
<br>
아래의 R명령어로 "아라가키 유이 갤러리" 전체를 크롤링 할 수 있습니다 <br>
<br>
첫 페이지를 크롤링한 것처럼 GET/POST와 셀레니움(Selenium)를 결합하였고<br>
<br>
다음 게시글에서는 "GET/POST만 활용하는 명령어"를 올리도록 하겠습니다<br>
<br>
 <br>
<br>
```R
##### 각키갤 <GET/POST + 셀레니움(Selenium)><br>

링크_각키 <- c()        ## "링크_각키" 라는 빈 벡터를 만듭니다 

n <- 201 

for(i in 1:n){ 
  tryCatch({ 
     
res_yui <- GET(url = 'https://gall.dcinside.com/mgallery/board/lists/', 
              query = list(id = 'aragakiyui', 
                           page=i)) 



cat(i, '페이지 수집 중. 상태코드는', status_code(x = res_yui), '입니다.\n') 



## 각키갤 링크 수집 

링크.각키.tmp <- res_yui %>% 
  read_html() %>%  
  html_nodes('td.gall_tit.ub-word') %>% 
  html_nodes('a:nth-child(1)') %>% 
  html_attr('href') %>%
  unique() 
   
if (length(링크_각키.tmp) == 0) { 
  링크_각키 <- append(링크.각키, "수동확인") 
} else { 
  링크_각키 <- append(링크_각키, 링크.각키.tmp) 
}  



Sys.sleep(time = 1)  #### (중요!) 반복되는 작업으로 디도스(DDOS)로 오인 받지 않을려면 반드시 넣습니다!!

  }, error = function(e) cat("불러올 수 없습니다!\n")) 
   
} 


링크_각키 <- paste0("https://gall.dcinside.com/",링크_각키) 





## RSelenium 

remDr <- remoteDriver(remoteServerAddr="localhost",  
                      port=4445L,  
                      browserName="chrome") 
remDr$open() 


제목_각키 <- c() 
작성자_각키 <- c() 
날짜_각키 <- c() 
본문_각키 <-c() 
주소_각키 <-c() 

for (i in 1:length(링크_각키)){ 
  tryCatch({ 

remDr$navigate(링크_각키[i]) 
body <- remDr$getPageSource()[[1]] 
body <- body %>% read_html() 

cat('현재', i, '페이지 수집 중! \n')  



## 제목 

제목.각키.tmp <- body %>%  
  html_nodes("span.title_subject") %>%  
  html_text() 

if (length(제목.각키.tmp) == 0) { 
  제목_각키 <- append(제목_각키, "수동확인") 
} else { 
  제목_각키 <- append(제목_각키, 제목.각키.tmp) 
}  



## 작성자 

작성자.각키.tmp <- body %>%  
  html_nodes("div.fl") %>%  
  html_nodes("span.nickname") %>%  
  html_nodes("em") %>% 
  html_text() 

if (length(제목.각키.tmp) == 0) { 
  작성자_각키 <- append(작성자_각키, "수동확인") 
} else { 
  작성자_각키 <- append(작성자_각키, 작성자.각키.tmp) 
}  



## 날짜 

날짜.각키.tmp <- body %>%  
  html_nodes("span.gall_date") %>%  
  html_text()

if (length(날짜.각키.tmp) == 0) { 
  날짜_각키 <- append(날짜_각키, "수동확인") 
} else { 
  날짜_각키 <- append(날짜_각키, 날짜.각키.tmp) 
}  


## 본문 

본문.각키.tmp <- body %>%  
  html_nodes("div.writing_view_box") %>%  
  html_text() 


if (length(본문.각키.tmp) == 0) { 
  본문_각키 <- append(본문_각키, "수동확인") 
} else { 
  본문_각키 <- append(본문_각키, 본문.각키.tmp) 
}  




## 주소(URL) 

주소_각키 <- append(주소_각키, 링크_각키[i]) 


Sys.sleep(time = 1)  #### (중요!) 반복되는 작업으로 디도스(DDOS)로 오인 받지 않을려면 반드시 넣습니다!!

  }, error = function(e) cat("불러올 수 없습니다!\n")) 
} 





## 데이터 전처리 

본문_각키 <- str_replace_all(본문_각키,"\n","") 
본문_각키 <- str_replace_all(본문_각키,"\t","") 



## 데이터프레임으로 합치기 및 csv파일로 저장 

df_각키 <- data.frame(제목_각키, 작성자_각키, 날짜_각키, 본문_각키, 주소_각키) 
write.csv(df_각키, file = "D:/df_각키.csv", row.names=FALSE) 
```
<br>
 <br>
<br>

<br>
<br><br>
결과로 나온 최종 CSV파일입니다<br>
![gaki14](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F32tAq%2FbtqBVgyT20U%2FC6B3QOq9721nzciXT4TNUk%2Fimg.png)
