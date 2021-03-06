---
title: R 크롤링 디시인사이드 (DC Inside) GET / POST 방식
author: Lee
date: 2020-02-10 13:25 +0900
categories: [R, Crawling]
tags: [GET crawling, POST crawling, r, r crawling, R language, R 크롤링, Selenium, 디시인사이드, 디시인사이드 크롤링, 셀레늄]
math: true
mermaid: true
image:
  src: 
---
<br>
![gaki1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcgJs0j%2FbtqBSPPn6L9%2FuNjqODoytq7e5KOgE3K6Sk%2Fimg.jpg)
<br>
<br>
<br>
<br>
```R
· library(dplyr)
· library(httr)
· library(jsonlite)
· library(rJava)
· library(RSelenium)
· library(stringr)

#### 각키갤 GET/POST방식 

링크_각키 <- c()  

n <- 201 

for(i in 1:n){ 
  tryCatch({ 
     
    res_yui <- GET(url = 'https://gall.dcinside.com/mgallery/board/lists/', 
                   query = list(id = 'aragakiyui', 
                                page=i)) 
     
     
     
    cat(i, '페이지 수집 중. 상태코드는', status_code(x = res_yui), '입니다.\n') 
     
     
     
    #### 각키갤 링크 수집 
     
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
    }  ## 수집한 링크가 없을경우 "수동확인", 제대로 수집한 경우는 "링크_각키"로 저장합니다  
     
     
     
    Sys.sleep(time = 1) #### (중요!) 반복되는 작업으로 디도스(DDOS)로 오인 받지 않을려면 반드시 넣습니다!!
     
  }, error = function(e) cat("불러올 수 없습니다!\n")) 
   
} 




## 링크 분할 

링크_각키_no <- c() 
링크_각키_page <- c() 



링크_각키 <- strsplit(링크_각키, split="&") 


for(i in 1:length(링크_각키)){ 
  링크_각키_no.tmp <- 링크_각키[[i]][2] 
  링크_각키_page.tmp <- 링크_각키[[i]][3] 
   
  링크_각키_no <- append(링크_각키_no, 링크_각키_no.tmp) 
  링크_각키_page <- append(링크_각키_page, 링크_각키_page.tmp) 
   
} 


링크_각키_no <- str_replace_all(링크_각키_no,"no=","") 
링크_각키_page <- str_replace_all(링크_각키_page,"page=","") 




## 페이지별 element 수집 

제목_각키 <- c() 
작성자_각키 <- c() 
날짜_각키 <- c() 
본문_각키 <-c() 
주소_각키 <-c()  



for(i in 1:length(링크_각키)){ 
  tryCatch({ 
     
res_yui <- GET(url = 'https://gall.dcinside.com/mgallery/board/view/', 
               query = list(id = 'aragakiyui', 
                            no = 링크_각키_no[i], 
                            page = 링크_각키_page[i])) 


cat(i, '페이지 수집 중. 상태코드는', status_code(x = res_yui), '입니다.\n') 




## 제목 

제목.각키.tmp <- res_yui %>% 
                 read_html() %>%   
                 html_nodes("span.title_subject") %>%  
                 html_text() 


if (length(제목.각키.tmp) == 0) { 
  제목_각키 <- append(제목_각키, "수동확인") 
} else { 
  제목_각키 <- append(제목_각키, 제목.각키.tmp) 
} 

 



## 작성자 

작성자.각키.tmp <- res_yui %>% 
                   read_html() %>%  
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

날짜.각키.tmp <- res_yui %>% 
                 read_html() %>%  
                 html_nodes("span.gall_date") %>%  
                 html_text() 


if (length(날짜.각키.tmp) == 0) { 
  날짜_각키 <- append(날짜_각키, "수동확인") 
} else { 
  날짜_각키 <- append(날짜_각키, 날짜.각키.tmp) 
}  

 



## 본문 

본문.각키.tmp <- res_yui %>% 
                 read_html() %>%  
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
