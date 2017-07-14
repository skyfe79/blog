---
id: 63
title: 'Genymotion 의 adb server is out of date. killing&#8230;'
date: 2014-11-07T05:25:08+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=63
permalink: '/genymotion-%ec%9d%98-adb-server-is-out-of-date-killing/'
dsq_thread_id:
  - "3714546599"
categories:
  - Android
tags:
  - adb
  - genymotion
---
롤리팝으로 SDK를 업데이트한 후 게니모션을 실행시켜서 adb 를 사용하려고 하면 아래와 같은 오류가 발생한다.

<pre class="lang:default decode:true ">$ adb shell
adb server is out of date.  killing...
cannot bind 'tcp:5037'
ADB server didn't ACK
* failed to start daemon *
error:</pre>

이것은 시스템에 설치된 안드로이드 SDK tools 와 Genymotion 의 SDK tools의 버전이 서로 달라서 생기는 문제다.  아래와 같이 현재 실행되고 있는 모든 adb 프로세스를 kill 한 다음 Genymotion 을 설정해 준다.

<pre class="lang:default decode:true ">$ killall -9 adb</pre>

[<img class="size-full wp-image-64 aligncenter" src="http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-07-오후-2.17.25.png?resize=608%2C609" alt="스크린샷 2014-11-07 오후 2.17.25" srcset="http://i0.wp.com/burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-07-오후-2.17.25.png?w=608 608w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-07-오후-2.17.25.png?resize=150%2C150 150w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-07-오후-2.17.25.png?resize=300%2C300 300w" sizes="(max-width: 608px) 100vw, 608px" data-recalc-dims="1" />](http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-07-오후-2.17.25.png)

Genymotion을 재실행한다.

<pre class="lang:default decode:true ">$ adb shell
root@vbox86p:/ #</pre>

&nbsp;