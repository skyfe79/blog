---
id: 406
title: JNI 튜토리얼
date: 2015-05-04T21:39:21+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=406
permalink: '/jni-%ed%8a%9c%ed%86%a0%eb%a6%ac%ec%96%bc/'
dsq_thread_id:
  - "3735016562"
categories:
  - Android
  - C++
  - Java
tags:
  - c
  - java
  - jni
---
안드로이드 NDK 프로그래밍을 하기 전에 Java와 C/C++을 잇는 JNI에 먼저 익숙해 지는 것이 먼저일 것이다. 틈나는 대로 예제 중심의 JNI 튜토리얼을 만들어 봐야 겠다. 생각나는 대로 적기에 올바른 순서는 없다. 개발환경은 MacOSX 이다.<!--more-->

  1. [HelloJNI](http://blog.burt.pe.kr/hellojni/ "HelloJNI")
  2. [메서드 등록](http://blog.burt.pe.kr/%eb%a9%94%ec%84%9c%eb%93%9c-%eb%93%b1%eb%a1%9d/ "메서드 등록")
  3. [int 데이터 값 주고 받기](http://blog.burt.pe.kr/int-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ea%b0%92-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/)
  4. [boolean 데이터 값 주고 받기](http://blog.burt.pe.kr/boolean-%ed%98%95-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ea%b0%92-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/)
  5. [double 데이터 값 주고 받기](http://blog.burt.pe.kr/double-%ed%98%95-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/)
  6. [float 데이터 값 주고 받기](http://blog.burt.pe.kr/float-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/)
  7. [long 데이터 값 주고 받기](http://blog.burt.pe.kr/long-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/)
  8. [short 데이터 값 주고 받기](http://blog.burt.pe.kr/short-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/)
  9. [char 데이터 값 주고 받기](http://blog.burt.pe.kr/char-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/)
 10. [byte 데이터 값 주고 받기](http://blog.burt.pe.kr/byte-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/)
 11. [JNI 문자열 1/3](http://blog.burt.pe.kr/jni-%eb%ac%b8%ec%9e%90%ec%97%b4-13/)
 12. [JNI 문자열 2/3](http://blog.burt.pe.kr/jni-%eb%ac%b8%ec%9e%90%ec%97%b4-23/)
 13. [JNI 문자열 3/3](http://blog.burt.pe.kr/jni-%eb%ac%b8%ec%9e%90%ec%97%b4-33/)
 14. 레퍼런스 이해하기
 15. 배열 다루기 &#8211; 1/4
 16. 배열 다루기 &#8211; 2/4
 17. 배열 다루기 &#8211; 3/4
 18. 배열 다루기 &#8211; 4/4