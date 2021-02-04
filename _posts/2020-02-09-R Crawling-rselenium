---
title: R 크롤링 RSelenium (셀레니움) 을 크롬에서 구동하기
author: Lee
date: 2020-2-9 15:42 +0900
categories: [R, Crawling]
tags: [chrome driver, cmd, gecko driver, Rselenium, Rselenium설치, Selenium, Selenium standalone server, selenium설치, 셀레니움]
math: true
mermaid: true
image:
  src: 
---
<br>
R에서 Selenium을 구동하려면 Java를 설치해야 합니다.<br>
<br>
Java설치는 https://blogleeyoo.github.io/r-pyomega/posts/R-java/ 를 참고해주시길 바랍니다<br>
<br>
Java설치 이후에 C드라이브에 r-selenium폴더를 만들어 아래 3파일을 다운 받습니다<br>
<br>
(폴더 이름을 r-selenium로 지정하였습니다. 이름은 상관없지만 가능하면 C드라이브에 지정하는것 편리합니다)<br>
<br>
**· Selenium standalone server**<br>
<br>
**· gecko driver**<br>
<br>
**· chrome driver<br>**
<br>
 <br>
<br>
 <br>
<br>
**1. Selenium이란?<br>**
<br>
Selenium의 정의는 구글링으로 찾아보실 수 있을겁니다.<br>
<br>
해당 분야 전문가가 아니라면 단순하게 셀레니움은 **"브라우저(browser) 자동화"**를 가능하게 하는 프로그램이라 생각하시면 됩니다. 굉장히 흉악한 성능을 발휘합니다.<br>
<br>
마우스로 브라우저를 움직이는 것처럼, 셀레니움은 경로를 지정함으로서 브라우저를 구동합니다.<br>
<br>
당장 이해가 되지 않더라도 괜찮습니다. 셀레니움이 움직이는 것을 눈으로 확인하면 금방 이해할수 있습니다<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
**2. Chrome에서 Selenium 구동에 필요한 도구 다운 받기<br>**
<br>
**1) Selenium standalone server 다운받기<br>**
<br>
http://selenium-release.storage.googleapis.com/index.html에 접속합니다<br>
<br> 
 <br>
<br>
 <br>
<br>
아래로 내려가서 4.0 폴더를 클릭합니다<br>
![Selenium standalone server](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FXutPh%2FbtqBRvQKIBx%2FxOV82m4xwk1IIl2cktvtB0%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
selenium-server-standalone-4.0.0-alpha-1.jar 파일을 클릭해서 r-selenium폴더에 저장합니다<br>
<br>
(예전에는 3.11버전을 사용했는데 성능에는 문제가 없었고 4.0.0버전도 문제없이 구동됩니다. 편하실대로 하시면 됩니다)<br>
![Selenium standalone server1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb7zOXc%2FbtqBS6ppJBk%2FCqBX3CKKw6cBZTE8kGMGmk%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
**2) geckodriver 다운받기<br>**
<br>
https://github.com/mozilla/geckodriver/releases/tag/v0.17.0에 접속합니다<br>
<br>
 <br>
<br>
 <br>
<br>
밑으로 쭉 내려가 Assets에서 운영체제에 맞는 파일을 다운받아 압축 파일을 r_selenium 폴더에 압축을 풀면됩니다
![Selenium standalone server2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmD5sD%2FbtqBPY7fnJr%2Fa5VL57GEtVZek10Gz4vlzk%2Fimg.png)
<br>
<br>
 <br>
<br>
**3) chrome driver 다운받기<br>**
<br>
https://sites.google.com/a/chromium.org/chromedriver/에 접속합니다<br>
 <br><br>
<br>
 <br>
<br>
다운로드 링크를 쉽게 찾을 수 있습니다<br>
<br>
클릭합니다<br>
![chrome driver](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbGoLut%2FbtqBRwPE2Cl%2FRXp4LsmkcdPLZVcxTkTjk1%2Fimg.png)
<br>
 <br>
<br>
 <br>
<br>
운영체제에 맞는 압축파일을 다운받습니다. 저는 windows를 다운받고 압축파일을 r_selenium에 풉니다.<br>
![chrome driver1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIDoAm%2FbtqBO65sP7y%2FfPdV9aaFLpwL71A7A0K9r1%2Fimg.png)
<br>
<br>
<br>
 <br>
<br>
 <br>
<br>
**3.  명령 프롬프트 실행<br>**
<br>
window는 cmd를 검색하면 명령프롬프트를 찾을 수 있습니다<br>
![chrome driver2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTmCG8%2FbtqBPXtDG5U%2FitFtIj1QyjbkdBGiBcd4zk%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
콘솔창에  <br>
<br>
**cd C:\r_selenium** **입력-실행**<br>
<br>
**java -Dwebdriver.gecko.driver="geckodriver.exe" -jar selenium-server-standalone-4.0.0-alpha-1.jar -port 4445** 를 차례로 **입력-실행**합니다<br>
<br>
 <br>
<br>
명령에 밑에 아래와 같은 Selenium Server is up and running on port 4445 를 확인하면 정상적으로 셀레니움을 사용할 수 있습니다<br>
![chrome driver3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCGFom%2FbtqCD9N5M1o%2FeW1Y0eK8gKuT6jQt2PsXGK%2Fimg.png)
<br>
