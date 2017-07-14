---
id: 139
title: 'Android Camera&#8217;s Bug'
date: 2014-11-11T09:43:34+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=139
permalink: /android-cameras-bug/
dsq_thread_id:
  - "3743142160"
categories:
  - Android
---
안드로이드 카메라에서 포커스 영역을 가져오는 메서드가 있다. 그러나 API 19(?) 이하에서는 NumberFormatException 예외가 발생하며 앱이 크래쉬 된다. 예외를 잡아서 처리할 수도 없고 난감하다.<!--more-->

<pre class="lang:java decode:true ">int maxNumFocusArea = parameters.getMaxNumFocusAreas();
if(maxNumFocusArea &gt; 0) {
    for (Camera.Area area : parameters.getFocusAreas()) {
        line(cameraAreaToString(area));
    }
}
</pre>

버그의 위치는 아래와 같다.

<pre class="lang:default decode:true ">I may have found the source of this bug in
https://android.googlesource.com/platform/hardware/qcom/camera/+/master/QCamera2/HAL/QCameraParameters.cpp

at line 524
#define DEFAULT_CAMERA_AREA "(0, 0, 0, 0, 0)"

Expected seems to be "(0,0,0,0,0)" which leads to the NumberFormatException when parsing an entry " 0" to an int in java.</pre>

  *  참고 
      * <https://code.google.com/p/android/issues/detail?id=61101>