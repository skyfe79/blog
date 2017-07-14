---
id: 254
title: ffmpeg으로 맥에서 녹화하기
date: 2015-02-13T12:09:55+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=254
permalink: '/ffmpeg%ec%9c%bc%eb%a1%9c-%eb%a7%a5%ec%97%90%ec%84%9c-%eb%85%b9%ed%99%94%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3732802001"
categories:
  - FFmpeg
  - Mac OS X
---
우선 맥에서 사용할 수 있는 비디오 장치 목록 조회하기

<pre class="lang:default decode:true ">ffmpeg -f qtkit -list_devices true -i ""</pre>

결과

<pre class="lang:default decode:true">[QTKit input device @ 0x7fbb93c26a20] QTKit video devices:
[QTKit input device @ 0x7fbb93c26a20] [0] FaceTime HD 카메라(내장형)</pre>

녹화하기

<pre class="lang:default decode:true ">ffmpeg -f qtkit -video_device_index 0 -i "" out.mpg</pre>

또는

<pre class="lang:default decode:true ">ffmpeg -f qtkit -i "default" out.mpg</pre>

&nbsp;