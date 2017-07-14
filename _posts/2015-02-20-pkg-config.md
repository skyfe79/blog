---
id: 279
title: pkg-config
date: 2015-02-20T23:22:53+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=279
permalink: /pkg-config/
dsq_thread_id:
  - "3730828840"
categories:
  - C++
  - Ubuntu
tags:
  - pkg-config
  - ubuntu
---
라이브러리를 이용하려면 -I, -l이나 -L처럼 컴파일 옵션을 알맞게 지정해야 한다. 하지만 포함된 파일이나 라이브러리를 일일이 지정하는 일은 꽤 성가신 작업이다. <!--more-->

pkg-config는 그런 불편함을 해결하고자 만들어졌다. pkg-config가 등장하기 전에는 개별 라이브러리에서 따로따로 명령을 제공했다. 예를 들어, GLib에는 옵션 설정을 표시하는 glib-config라는 명령이 있다. 하지만 그렇게 하면 낭비적인 요소가 많으므로 개개의 명령을 통합해 pkg-config를 만든 것이다.

pkg-config에 &#8211;list-all 옵션을 지정해서 실행해보자. 설치된 개발 패키지 중 해당 명령에 대응하는 목록이 표시되므로 pkg-config로 무엇을 할 수 있는지 확인할 수 있다.

glib-2.0을 사용하는 코드를 컴파일하려고 할 때 적절한 헤더파일 위치와 라이브러리를 설정해 줘야 한다. 이 것을 pkg-config로 알 수 있다.

<pre class="lang:default decode:true ">$ pkg-config glib-2.0 --cflags --libs

&lt;결과&gt;
-I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include  -lglib-2.0</pre>

pkg-config의 출력을 명령줄에서 이용하려면 역따옴표(\`)를 사용한다. 역따옴표로 에워싼 부분은 명령 실행에 앞서 실행되고, 그 결과가 명령줄에 전개된다.

<pre class="lang:default decode:true ">$ gcc example.c -o example `pkg-config glib-2.0 --cflags --libs`</pre>

출처

  * 우분투 환경에서 C언어로 배우는 리눅스 프로그래밍, page 157, 159