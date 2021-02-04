---
title: R 다변량 통계 분석 - R 크롤링 rjava 설치하기
author: Lee
date: 2020-2-9 14:32 +0900
categories: [R, Crawling]
tags: [Kolmogorov-Smirnov test, Multivariate, Normality Test, Q-Q plot, qqplotr, Shapiro-Wilk test, 다변량, 다변량 통계, 일변량, 정규성 검정]
math: true
mermaid: true
image:
  src: 
---
<br>
**1. Java 설치**<br>
<br>
R language를 사용하기 위해서 종종 Java가 필요할 때가 있습니다<br>
<br>
Selenium이나 한글 자연어 분석 package인 KoNLP을 사용할 때도 필수입니다<br>
<br>
(package는 R의 특정 기능을 활용하기 위한 "도구모음"이라고 생각하시면 됩니다)<br>
<br>
그럼 Java를 다운 받습니다<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
**1) Java 다운받기**<br>
<br>
https://www.java.com/ko/download/manual.jsp 에 접속하여 Java를 다운받습니다<br>
<br>
<br>
 <br>
<br>
본인 사용하는 PC의 운영체제에 맞는 Java를 다운받으면 됩니다.  32-bit는 Windows 온라인 또는 Windows 오프라인을 다운받고 64-bit 체제는 반드시 Windows 오프라인 (64비트) 설치해야 합니다<br>
![java](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbdIgO6%2FbtqBRvb7g0g%2FPb6SBYfmzKVAJxJbdkvWbk%2Fimg.png)
![64bit](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbSY124%2FbtqBO74lsVY%2Fkse6p2DKK1r5iPZUVoXInK%2Fimg.png)
<br>
 <br>
<br>
 <br>
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
**2) java 설치**<br>
<br>
java 설치는 어렵지 않게 하실수 있습니다. 설치가 끝났으면 C:\Program Files로 가서 Java 폴더가 있는지 확인합니다<br>
![java ins](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbMAoHK%2FbtqBQ2gXWh6%2FiG2fwJgKlrl9fkU3zMSiU0%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
다운 받은 Java를 설치하였으면 C:\Program Files\Java\jre1.8.0_241 경로를 확인할 수 있습니다. 저는 jre1.8.0_241버전을 다운 받았습니다. (다운받은 버전에 따라 경로는 다를수 있습니다. 버전은 달라도 상관없습니다) <br>
![java ins1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FboKoUK%2FbtqBODPXZ28%2FhGlXHs3sNrqh6Owpkzk6o0%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
**2. Java 경로지정**<br>
<br>
운영체제를 확인하는 화면으로 가서 고급 시스템 설정으로 갑니다.<br>
![java ins2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlcikE%2FbtqBNMNlycA%2F1bvma9wCaK53KjK7IpTwY0%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
환경 변수(N)를 클릭합니다<br>
![java ins3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FMElxi%2FbtqBPXAtGit%2F8aGjhIisWlH8BF22iTmmr0%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
시스템 변수(S)에서 새로 만들기(W)를 클릭합니다<br>
![java ins4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbd8M5P%2FbtqBOsVsLiY%2FwMSvAAt1D1JNPQGcY0h041%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
변수 이름에는 JAVA_HOME을 입력하고<br>
<br>
변수 값에는 Java를 설치한 경로인 C:\Program Files\Java\jre1.8.0_241 를 입력합니다<br>
![java ins5](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJ4LXU%2FbtqBO8CaeXK%2FlTr55cSbxpdqPSJWOFi0sk%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
사용자 변수에서 Path를 선택하고 편집을 클릭합니다<br>
![java ins6](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FudFAW%2FbtqBQ1h12uG%2FH7uha5TGai8P0VmqM3BbBk%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
위 경로를 선택하고 편집(E)를 클릭합니다<br>
![java ins7](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FedZXS6%2FbtqBO7XA9Cc%2FzUpzraVVZFgdaFHuO1FfM0%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
세미콜론(;)를 앞에 붙이고 설치한 Java경로인 C:\Program Files\Java\jre1.8.0_241를 입력합니다<br>
![java ins8](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcMowwY%2FbtqBNMUamS6%2FtNSSn7zVs9tTC07HjApW4k%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
Rstudio를 실행하고 **Sys.setenv(JAVA_HOME='C:/Program Files/Java/jre1.8.0_241') 명령어**를 입력하고 실행하면 Java설치와 경로지정이 끝나게 됩니다<br>
![java ins9](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbtijU1%2FbtqBSPOWHWo%2FJ71kIzOfW6RzxOJi5H7wQ0%2Fimg.png)
<br>
<br>
 <br>
<br>
 <br>
<br>
 <br>
<br>
참고로 rjava 패키지와 라이브러리를 불러오는 명령어는 아래와 같습니다<br>
<br>
저는 습관적으로 Rstuio를 불러올 때 마다 실행하니 참고바랍니다 <br>
![java ins9](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F3K9z3%2FbtqBOCwK6Y5%2FsZ0hzqnJREUPnFCfb1lE6k%2Fimg.png)
<br>


 
