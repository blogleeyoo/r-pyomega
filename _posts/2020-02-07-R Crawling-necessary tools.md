title: R 크롤링 필요 도구들 (Web crawling with R and necessary tools)
author: Lee
date: 2020. 2. 7. 17:53 +0900
categories: [R, Crawling]
tags: [Crawling, r, R language, R studio, 설치, 웹 크롤링, 크롤링, 필수]
math: true
mermaid: true
image:
  src: 
---
1. 웹 크롤링이란?<br>
웹 크롤링은 웹 페이지에서 보이는 data(contents라고도 합니다)를 유저의 편의에 따라 수집하는 것을 말합니다<br>
웹 크롤링 방식과 사용도구에 따라 크롤링 방식(process)은 조금씩 상이할 수 있지만 대체적으로 아래와 같은 방식을 따릅니다<br>
<br>
<br>
<br>
(1) HTTP Request(요청)<br>
· GET / POST 방식의 HTTP 통신 또는 RSelenium(셀레늄)<br>
<br>
<br>
<br>
(2) HTTP Response(응답)<br>
· 응답 결과 확인(상태코드로 확인가능합니다)<br>
<br>
· 응답 받은 객체를 텍스트로 출력하여 수집하려는 데이터인지 확인<br>
<br>
<br>
<br>
(3) 데이터 추출 및 전처리<br>
· 응답 받은 객체를 HTML로 변환<br>
<br>
· CSS / XPath를 이용하여 HTML 요소 위치 설정<br>
<br>
· 설정한 HTML 요소 위치에서 수집하려는 데이터 추출 (주로, 텍스트 / 링크 / 이미지)<br>
<br>
· 텍스트 / 링크는 전처리(예 : 불필요한 부분 제거) 후 편의에 따라 저장 (csv, xlsx)<br>
<br>
<br>
<br>
웹 크롤링을 처음 시작하시는 분들은 모르는 용어가 많아 감 잡기도 어려울수 있습니다.<br>
(글쓴이도 마찬가지) 특히 HTML관련 용어는 데이터 과학을 하셨던 분들이라도 생소하실껍니다.<br>
쉽게 말해 웹 크롤링의 프로세스는 손님이 데이터를 요청하고 <br>
서버가 주문에 따라 해당 객체 내어주는 패스트푸드점 유사하다고 할 수 있겠습니다<br>

![browser server](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcjgvOX%2FbtqBNMmjOPV%2FNoNtOI6vYXvmRwiiGmTPs1%2Fimg.png)
출처 : http://blog.daum.net/dosman1/2173<br>


경험상 웹 크롤링은 R에 능숙한 사람보다는 HTML을 잘 아시는 분이 더 빨리 배우시는 것 같습니다<br>
그렇지만 관련 지식이 없어도 몇번 실습을 해보시면 충분히 감을 잡을 수 있습니다<br>
역시 해보는게 중요하죠<br>
<br>
<br>
<br>
2. 웹 크롤링의 필요성<br>
웹 크롤링은 자동화시스템을 기본 전제로한 기술입니다<br>
굳이 자동화의 중요성을 말하지 않아도 아실겁니다<br>
예를 들어 2020년 1월 "손흥민" 관련 기사를 수집하고 싶다면 기사 페이지를 열어 마우스로 복사+붙여넣기를 하면 됩니다<br>
하지만... 네이버뉴스 기분 20년 1월 "손흥민" 관련기술은 "3,706"건 입니다.<br>
(크롤링을 하시다 보면 아시겠지만 이것도 사실 많은건 아닙니다)<br>
기사 하나의 본문을 복붙하는데 30초씩 잡아도 1,353분이나 걸립니다 ㅡㅡ...<br>
이러한 단순노동 작업을 우리는 코드 몇줄로 컴퓨터에게 맡겨서 시간절약을 하자는데 의미가 있다고 해야할까요<br>
아무튼 흔히 말하는 빅데이터를 수집하기 위해서는 결국 자동화 시스템 없이는 사실상 불가능합니다<br>
<br>
<br>
<br>
3. 필요 도구 설치 (R Language / R Studio)<br>
크롤링에 사용하는 도구는 다양합니다 ( R언어 / Python / Javascript / C언어 / Selenium / 정규표현식 등)<br>
저는 3가지 도구를 사용하고 있으며 이를 적절하게(좋게 말하면 임기응변, 나쁘게 말하면 야매...) 사용할 것입니다<br>
바둑으로 치면 정석이라기 보단 실전적인 꼼수에 가까울 수도 있겠으나... 아무튼 제가 사용한 도구는 아래와 같습니다<br>
<br>
· R language & Studio<br>
<br>
· RSelenium<br>
<br>
· 정규표현식<br>
<br>
그럼 다음 게시글에서 각 도구들의 설치방법을 알아보겠습니다<br>
<br>
<br>
