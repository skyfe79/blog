---
id: 287
title: Macbook Air(2013 mid)에 우분투 설치하기
date: 2015-02-26T12:28:44+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=287
permalink: '/macbook-air2013-mid%ec%97%90-%ec%9a%b0%eb%b6%84%ed%88%ac-%ec%84%a4%ec%b9%98%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3716809216"
categories:
  - Ubuntu
tags:
  - "2013"
  - macbook air
  - mid
  - ubuntu
---
리눅스 머신을 갖기 위해 맥북에어 2013 mid 에 우분투를 설치했다. 설치하다 만난 문제점과 해결 방법을 정리한다.

  * Prepare to install 에서 멈추는 현상 
      * 우분투를 설치할 때 14.04 LTS 버전을 바로 설치하면 설치 준비 중 화면에서 무한 대기를 한다.
      * 우분투 12.04 버전을 설치한 다음 14.04 로 업그레이드 한다. 이 방법이 가장 확실하다
  * 틸드(\`) 키가 < 키로 맵핑되는 버그 
      * 터미널을 열고 아래와 같이 입력한 후 실행한다. <pre class="lang:default decode:true ">&gt;&gt; sudo su -
&gt;&gt; echo 0 &gt; /sys/module/hid_apple/parameters/iso_layout</pre>
        
        &nbsp;</li> </ul> </li> </ul>