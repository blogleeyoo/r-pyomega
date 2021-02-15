---
title: R 크롤링 클리앙(Clien) GET / POST + Selenium (셀레니움)
author: Lee
date: 2020-02-11 17:01 +0900
categories: [R, Crwaling]
categories: [R, Crawling]
tags: [clien, GET crawling, r, r crawling, R language, R 크롤링, Selenium, 셀레니움, 클리앙, 클리앙 크롤링]
math: true
mermaid: true
image:
  src: 
---
<br>
![galaxy01](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcpT0w9%2FbtqBS5SVo1x%2Fk5RiKEutamV02qAukYsU7K%2Fimg.jpg)
<br>
클리앙에서 "갤럭시 폴드" 통합검색 결과를 크롤링 해보겠습니다
<br>
크롤링에 필요한 패키지(package)와 라이브러리(library)는 아래와 같습니다
 <br>
 
```R
install.packages(c("dplyr", "httr", "jsonlite", "rJava", "RSelenium", "stringr")

· library(dplyr)
· library(httr)
· library(jsonlite)
· library(rJava)
· library(RSelenium)
· library(stringr)
```

<br>
<br>
클리앙에 접속하여 통합검색 창에 "갤럭시 폴드"를 검색합니다
<br>
https://www.clien.net/service/search?q=갤럭시%20폴드
![clien01](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbz5NB5%2FbtqBTMS3myf%2FOOjPU3G7QakJTSNcRjssdK%2Fimg.png)
<br>
<br>
https://www.clien.net/service/search?q=갤럭시%20폴드
<br>
URL을 분리해봅시다
<br>
 <br>
<br>
URL : https://www.clien.net/service/search
<br>
Query String(추가적 정보) :
<br>
1) q=갤럭시%20폴드
<br>
 <br>
<br>
어떤 구조인지는 알겠는데 뭔가 부족한 느낌을 지울수 없습니다
<br>
그러면 2번째 페이지로 가봅시다
<br>
https://www.clien.net/service/search?q=갤럭시%20폴드&sort=recency&p=1&boardCd=&isBoard=false
![clien02](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcHyYVf%2FbtqBVeBi6br%2FwXFNgFDeUZuVdMCH5AD9W0%2Fimg.png)
<br>
URL에 추가적인 꼬리가 더 붙었습니다 다시 분리해봅시다
<br>
 <br>
<br>
URL : https://www.clien.net/service/search
<br>
Query String(추가적 정보) : "&"로 분리
<br>
1) q=갤럭시%20폴드            #### 검색어
<br>
2) sort=recency              #### sort를 recency로 합니다(즉, 최신순 정렬이죠)
<br>
3) p=1                       #### 첫번째 페이지란 뜻입니다 Query String에서 p는 페이지 정보일때가 많습니다
<br>
4) boardCd=
<br>
5) isBoard=false
<br>
<br>
다시 첫번째 페이지로 돌아가봅시다
<br>
https://www.clien.net/service/search?q=갤럭시%20폴드&sort=recency&p=0&boardCd=&isBoard=false
URL이 뭔가 달라졌습니다 다시 분리 해봅시다
<br> 
<br>
URL : https://www.clien.net/service/search
<br>
Query String(추가적 정보) :    "&"로 분리
<br>
1) q=갤럭시%20폴드              #### 검색어
<br>
2) sort=recency                #### 최신순 정렬
<br>
3) p=0                         #### 0번째 페이지란 뜻입니다
<br>
4) boardCd=
<br>
5) isBoard=false
<br>
<br>
클리앙은 독특하게 1페이지 정보가 0입니다
<br>
그래도 크롤링에는 문제가 없습니다
<br>
 <br>
F12를 눌러봅시다
![clien03](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnuzP2%2FbtqBVM5F5NE%2FNqK7IQ7yfItVWgeaFTU5MK%2Fimg.png)
<br>
<br>
가장 위 정보를 클릭합시다
<br>
찾지 못할경우 페이지 새로고침(F5)를 누르십시요
<br>
Header - General에서 필요한 정보는 Request URL과 Request Method입니다
<br>
아래로 내려가 Query String Parameter로 가서 정보를 확인합니다
![clien04](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbu7qQE%2FbtqBSOw6Bfu%2FokKgZR9MkcujB5vhJbZ9h0%2Fimg.png)
<br>
이 정보를 바탕으로 HTTP 요청 및 응답 명령어(code)를 작성합니다
<br>
<br>
### HTTP 요청 및 응답
```R
searchword <- "갤럭시 폴드"

 

  res_cl <- GET(url = "https://www.clien.net/service/search", 
                query = list(q = searchword, 
                             sort = "recency", 
                             p = 0, 
                             boardCd = "",              ## boardCd: 에는 정보가 없는데, ""로 처리하면 됩니다
                             isBoard = "false")
```
<br>
<br>
크롤링의 큰 그림을 설명하겠습니다
<br>
1) 해당 게시글을 링크를 수집한다 (중요)
<br>
2) 수집한 링크에서 데이터(요소 : Element)를 찾아서 수집한다
<br>
<br>
<br>
좀 더 풀어서 설명하면
<br>
1) 1페이지의 링크를 수집
<br>
2) 수집한 링크를 타고가서 해당요소를 수집
<br>
3) for문(자동화)으로 검색한 결과 전체 크롤링
<br>
<br>
<br>
그러면 1)에 따라 링크를 수집합니다
<br>
링크를 수집하기 위해 element를 확인합니다
<br>
F12에서 보이는 제일 왼쪽 위 아이콘(Ctrl + Shift + C)을 클릭하고 게시글 제목을 클릭합니다
![clien05](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPJJ8a%2FbtqBQ1wZW7A%2FVYB2ILG5eyJTaPiwYd9FJ0%2Fimg.png)
<br>
위 정보를 바탕으로 링크를 수집해봅시다
<br>
## 링크
```R
링크_cl <- c()                                                     ## 수집한 링크를 담을 빈 벡터 공간을 만듭니다

   
  링크.tmp <- res_cl %>%  
    read_html() %>%  
    html_nodes("a.subject_fixed") %>%        ##링크 주소 바로 위에 있는 Element입니다 
    html_attr("href") %>%                              ## href="링크주소"를 긁는 명령어입니다
    unique()                                                       ##중복되는 링크를 제거해주는 함수입니다
  

  링크.tmp <- paste0("https://www.clien.net",링크.tmp)    ### 수집한 링크 앞에 "https://www.clien.net"를 넣어줍니다

   
  링크_cl <- append(링크.tmp,링크_cl)  
 ```
<br>
<br>
링크 수집을 완료하였습니다
<br>
이번에는 링크를 타고 들어가기 전에 날짜도 수집합시다
<br>
F12에서 보이는 제일 왼쪽 위 아이콘(Ctrl + Shift + C)을 클릭하고 게시글 날짜를 클릭합니다
![clien06](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdFihUD%2FbtqBUAdWHIy%2FKpxdNs0WV0MNtONn3h3I8k%2Fimg.png)
<br>
<br>
element정보를 보니 
<br>
span.time popover에는 13:29
<br>
span.timestamp에는 2020-02-11 13:29:49가 있습니다
<br>
날짜까지 포함한 element인 후자를 선택합시다
<br>
<br>
##날짜
```R
날짜_cl <- c()                                                   ## 수집한 날짜를 담을 빈 벡터 공간을 만듭니다

   
  날짜.tmp <- res_cl %>%  
    read_html() %>%  
    html_nodes("span.timestamp") %>% 
    html_text()  
   
  if (length(날짜.tmp) == 0) { 
    날짜_cl <- append(날짜_cl, "수동확인") 
  } else { 
    날짜_cl <- append(날짜_cl, 날짜.tmp) 
  }
 ```
 <br>
 <br>
 그럼 이제 수집한 링크를 타고가서
<br>
"제목", "본문", "조회수", "댓글수" 그리고 이번엔 "댓글"까지도 크롤링합시다
<br>
크롤링한 페이지의 링크는 따로 저장하기 위해 "주소"로 저장합니다 
<br>
 <br>
<br>
크롤링 하기로한 element 이름으로 빈 벡터공간을 만듭니다
```R
제목_cl <- c()

본문_cl <- c()

조회수_cl <- c()

댓글수_cl <- c()

댓글_cl <- c() 

주소_cl <- c()
```
<br>
<br>
그리고 셀레니움을 켭니다
<br>
명령프롬프트를 키고
```
1) cd C:\r_selenium 
2) java -Dwebdriver.gecko.driver="geckodriver.exe" -jar selenium-server-standalone-4.0.0-alpha-1.jar -port 4445를 입력합니다
```
<br>
<br>
<br>
## RSelenium
```R
remDr <- remoteDriver(remoteServerAddr="localhost", 
                      port=4445L, 
                      browserName="chrome")
remDr$open()
```
링크와 게시글 제목을 수집한것 처럼
<br>
F12에서 보이는 제일 왼쪽 위 아이콘(Ctrl + Shift + C)을 클릭하고 수집하려는 element를 클릭합니다
<br>
 <br>
<br>
스크린샷은 생략하겠습니다
 <br>
아래는 element를 수집하는 명령어입니다
<br>
사전에 수집한 링크의 첫번째 페이지를 수집합니다
```R
링크_cl[1] = https://www.clien.net/service/board/kin/14581236?combine=true&q=%EA%B0%A4%EB%9F%AD%EC%8B%9C+%ED%8F%B4%EB%93%9C&p=0&sort=recency&boardCd=&
```
<br>
```R
remDr$navigate(링크_cl[1])                          ## 링크_cl[1]에 할당된 URL를 탐색합니다
body <- remDr$getPageSource()[[1]]          ## URL page Source를 가져옵니다
body <- body %>% read_html()                  ## html를 읽어들입니다
```
<br>
<br>
##제목
```R
    제목.tmp <- body %>%  
      html_nodes("h3.post_subject") %>%  
      html_text() 
       
    if (length(제목_cl) == 0) { 
      제목_cl <- append(제목_cl, "수동확인") 
    } else { 
      제목_cl <- append(제목_cl, 제목.tmp) 
    }
 ```
 <br>
 <br>
 ##본문
 ```R
     본문.tmp <- body %>%  
      html_nodes("div.post_article") %>%  
      html_text() 
     
    if (length(본문_cl) == 0) { 
      본문_cl <- append(본문_cl, "수동확인") 
    } else { 
      본문_cl <- append(본문_cl, 본문.tmp) 
    }
```
<br>
<br>
##조회수
```R
조회수.tmp <- body %>%  
      html_nodes("span.view_count") %>%  
      html_text() 
     
    if (length(조회수_cl) == 0) { 
      조회수_cl <- append(조회수_cl, "수동확인") 
    } else { 
      조회수_cl <- append(조회수_cl, 조회수.tmp) 
    }
```
<br>
<br>
##댓글수
```R
댓글수.tmp <- body %>%  
      html_nodes('div.comment_head') %>% 
      html_text() 
     
    if (length(댓글수_cl) == 0) { 
      댓글수_cl <- append(댓글수_cl, "수동확인") 
    } else { 
      댓글수_cl <- append(댓글수_cl, 댓글수.tmp) 
    }
```
<br>
<br>
##댓글
```R
댓글.tmp <- body %>%  
      html_nodes("div.comment_view") %>%  
      html_text() 
     
    if (length(댓글_cl) == 0) { 
      댓글_cl <- append(댓글_cl, "수동확인") 
    } else { 
      댓글_cl <- append(댓글_cl, 댓글.tmp) 
    } 
```
<br>
<br>
##주소
```R
주소_cl <- append(주소_cl, 링크_cl[1])
```
<br>
<br>
수집한요소를 전처리 합시다
```R
제목_cl <- gsub("\n", "", 제목_cl) 
제목_cl <- gsub("\t", "", 제목_cl) 

본문_cl <- gsub("\n", "", 본문_cl) 
본문_cl <- gsub("\t", "", 본문_cl) 

조회수_cl <- gsub("\\D", "", 조회수_cl)             ## "\\D"은 숫자가 아닌 문자를 선택하는 정규표현식입니다

댓글_cl <- gsub("\n", "", 댓글_cl) 
댓글_cl <- gsub("\t", "", 댓글_cl) 

댓글수_cl <- gsub("\\D", "", 댓글수_cl)
```
<br>
<br>
수집한 element를 df로 만들어 csv파일로 저장합니다
```R
clien_갤럭시폴드_본문 <- data.frame(날짜_cl, 제목_cl, 본문_cl, 조회수_cl, 댓글수_cl, 주소_cl)   
clien_갤럭시폴드_댓글 <- data.frame(댓글_cl)  

write.csv(clien_갤럭시폴드_본문, file = "D:/clien_갤럭시폴드_본문.csv", row.names=FALSE) 
write.csv(clien_갤럭시폴드_댓글, file = "D:/clien_갤럭시폴드_댓글.csv", row.names=FALSE)
```
<br>
<br>
## for문으로 자동화하기(중요!!)
<br>
첫 페이지 크롤링을 모두 완료하였습니다
<br>
for문과 tryCatch( )를 사용하여 모든 페이지를 크롤링합니다
<br>
아래의 R명령어로 검색결과 전체를 크롤링 및 전처리, csv로 저장 할 수 있습니다 
<br>
```R
#####링크 수집


n <- 50 
searchword <- "갤럭시 폴드" 

링크_cl <- c() 
날짜_cl <- c() 

for(i in 0:n){ 
  tryCatch({ 
  res_cl <- GET(url = "https://www.clien.net/service/search", 
                query = list(q = searchword, 
                             sort = "recency", 
                             p = i, 
                             boardCd = "", 
                             isBoard = "false")) 
   
  cat('현재', i, '페이지 수집 중! 상태코드는', status_code(x = res_cl), '입니다.\n') 
   
  ##링크 
   
  링크.tmp <- res_cl %>%  
    read_html() %>%  
    html_nodes("a.subject_fixed") %>% 
    html_attr("href") %>% 
    unique() 
   
  링크.tmp <- paste0("https://www.clien.net",링크.tmp) 
   
  링크_cl <- append(링크.tmp,링크_cl)  
   
   
   
  ##날짜 
   
  날짜.tmp <- res_cl %>%  
    read_html() %>%  
    html_nodes("span.timestamp") %>% 
    html_text()  
   
  if (length(날짜.tmp) == 0) { 
    날짜_cl <- append(날짜_cl, "수동확인") 
  } else { 
    날짜_cl <- append(날짜_cl, 날짜.tmp) 
  } 
   
   
  Sys.sleep(time = 1)  ## (중요!) 반복되는 작업으로 디도스(DDOS)로 오인 받지 않을려면 반드시 넣습니다!!
   
  }, error = function(e) cat("불러올 수 없습니다!\n")) 
   
} 




## 명령프롬프트 실행


#cd C:\r_selenium 
# java -Dwebdriver.gecko.driver="geckodriver.exe" -jar selenium-server-standalone-4.0.0-alpha-1.jar -port 4445 

 


## RSelenium 

remDr <- remoteDriver(remoteServerAddr="localhost",  
                      port=4445L,  
                      browserName="chrome") 
remDr$open() 



## 제목 본문 댓글 조회수 댓글수 주소 

제목_cl <- c() 
본문_cl <- c() 
댓글_cl <- c() 
조회수_cl <- c() 
댓글수_cl <- c() 
주소_cl <- c() 


  for (i in 403:length(링크_cl)){ 
  tryCatch({ 
    remDr$navigate(링크_cl[i]) 
    body <- remDr$getPageSource()[[1]] 
     
    cat('현재', i, '페이지 수집 중! \n')  
     
    body <- body %>% read_html() 
     
     
    ##제목 
     
    제목.tmp <- body %>%  
      html_nodes("h3.post_subject") %>%  
      html_text() 
       
    if (length(제목_cl) == 0) { 
      제목_cl <- append(제목_cl, "수동확인") 
    } else { 
      제목_cl <- append(제목_cl, 제목.tmp) 
    } 
     
     
    ##본문 
     
    본문.tmp <- body %>%  
      html_nodes("div.post_article") %>%  
      html_text() 
     
    if (length(본문_cl) == 0) { 
      본문_cl <- append(본문_cl, "수동확인") 
    } else { 
      본문_cl <- append(본문_cl, 본문.tmp) 
    } 
     
     
     
    ##댓글 
     
    댓글.tmp <- body %>%  
      html_nodes("div.comment_view") %>%  
      html_text() 
     
    if (length(댓글_cl) == 0) { 
      댓글_cl <- append(댓글_cl, "수동확인") 
    } else { 
      댓글_cl <- append(댓글_cl, 댓글.tmp) 
    } 
     
     
    ##조회수 
     
    조회수.tmp <- body %>%  
      html_nodes("span.view_count") %>%  
      html_text() 
     
    if (length(조회수_cl) == 0) { 
      조회수_cl <- append(조회수_cl, "수동확인") 
    } else { 
      조회수_cl <- append(조회수_cl, 조회수.tmp) 
    } 
     
     
    ##댓글수 
     
    댓글수.tmp <- body %>%  
      html_nodes('div.comment_head') %>% 
      html_text() 
     
    if (length(댓글수_cl) == 0) { 
      댓글수_cl <- append(댓글수_cl, "수동확인") 
    } else { 
      댓글수_cl <- append(댓글수_cl, 댓글수.tmp) 
    } 
     
     
     
    ##주소 
     
    주소_cl <- append(주소_cl, 링크_cl[i]) 
     
     
     
    Sys.sleep(time = 1)     ## (중요!) 반복되는 작업으로 디도스(DDOS)로 오인 받지 않을려면 반드시 넣습니다!!
     
  }, error = function(e) cat("불러올 수 없습니다!\n")) 
} 
  
  
  
## 데이터 전처리
 
제목_cl <- gsub("\n", "", 제목_cl) 
제목_cl <- gsub("\t", "", 제목_cl) 

본문_cl <- gsub("\n", "", 본문_cl) 
본문_cl <- gsub("\t", "", 본문_cl) 

조회수_cl <- gsub("\\D", "", 조회수_cl) 

댓글_cl <- gsub("\n", "", 댓글_cl) 
댓글_cl <- gsub("\t", "", 댓글_cl) 

댓글수_cl <- gsub("\\D", "", 댓글수_cl) 



## data.frame 저장

clien_갤럭시폴드_본문 <- data.frame(날짜_cl, 제목_cl, 본문_cl, 조회수_cl, 댓글수_cl, 주소_cl)   
clien_갤럭시폴드_댓글 <- data.frame(댓글_cl)  

write.csv(clien_갤럭시폴드_본문, file = "D:/clien_갤럭시폴드_본문.csv", row.names=FALSE) 
write.csv(clien_갤럭시폴드_댓글, file = "D:/clien_갤럭시폴드_댓글.csv", row.names=FALSE) 
```


최종결과로 csv저장된 파일입니다
![clien07](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3fG0z%2FbtqBUUce1bh%2FQHYfEs877ZzUudRQlHQVN0%2Fimg.png)
클리앙 : 날짜 제목 본문 조회수 댓글수 주소
![clien08](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbiGClR%2FbtqBVMrfgnx%2FlkbYyOA6gn0fLPsNHY2D80%2Fimg.png)
클리앙 : 댓글

다음 포스팅은 GET/POST방법으로만 클리앙(Clien)을 크롤링 하겠습니다

