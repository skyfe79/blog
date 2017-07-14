---
id: 554
title: Hello, PercentLayout!
date: 2015-08-21T14:46:23+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=554
permalink: /hello-percentlayout/
dsq_thread_id:
  - "4052592162"
categories:
  - Android
tags:
  - Percent Layout
---
안드로이드 서포트 라이브러리가 23 버전으로 업데이트 되었습니다. 업데이트 내용 중에 PercentLayout이 있는데 이 레이아웃은 퍼센트로 뷰의 너비와 높이를 정할 수 있게 해 줍니다. 즉, 안드로이드의 다양한 디바이스 화면 크기에 상관 없이 가로, 세로 모두 100% 로 정하는 것입니다. PercentLayout은 아래와 같이 두 개의 레이아웃이 있습니다.

  * PercentRelativeLayout
  * PercentFrameLayout

PercentLayout을 사용하기 위해서는 안드로이드 서포트 라이브러리와 안드로이드 빌드 도구를 23.0.0 버전으로 업데이트 해야 합니다.

[<img class=" size-full wp-image-555 aligncenter" src="http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/08/percent01.png?resize=665%2C408" alt="percentLayout" srcset="http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/08/percent01.png?w=902 902w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/08/percent01.png?resize=300%2C184 300w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/08/percent01.png?resize=660%2C405 660w" sizes="(max-width: 665px) 100vw, 665px" data-recalc-dims="1" />](http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/08/percent01.png)

&nbsp;

그 다음 앱 모듈의 build.gradle 파일을 아래와 같이 설정해 줍니다.

<pre class="lang:default mark:5,25 decode:true">apply plugin: 'com.android.application'

android {
    compileSdkVersion 22
    buildToolsVersion "23.0.0"

    defaultConfig {
        applicationId "kr.pe.burt.hellopercentlayout"
        minSdkVersion 18
        targetSdkVersion 22
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:22.2.1'
    compile 'com.android.support:percent:23.0.0'
}</pre>

이제 PercentLayout을 사용할 수 있습니다. 아래와 같이 PercentRelativeLayout 을 사용해 레이아웃을 작성할 수 있습니다.

<pre class="lang:xhtml mark:1,3,16,17,26,27,30 decode:true">&lt;android.support.percent.PercentRelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"&gt;


    &lt;ImageView
        android:layout_alignParentLeft="true"
        android:layout_alignParentTop="true"
        android:src="@drawable/ic_launcher"
        android:scaleType="fitXY"
        android:background="#f00"
        app:layout_widthPercent="50%"
        app:layout_heightPercent="50%"
        /&gt;

    &lt;ImageView
        android:layout_alignParentRight="true"
        android:layout_alignParentBottom="true"
        android:src="@drawable/ic_launcher"
        android:scaleType="fitXY"
        android:background="#f00"
        app:layout_widthPercent="50%"
        app:layout_heightPercent="50%"
        /&gt;

&lt;/android.support.percent.PercentRelativeLayout&gt;
</pre>

참고 : PercentLayout 내에서도 layout\_width와 layout\_height를 사용할 수 있습니다.

위 레이아웃을 실행하면 아래와 같이 화면이 구성됩니다.

[<img class=" size-full wp-image-562 aligncenter" src="http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/08/percent021.jpeg?resize=400%2C533" alt="percentLayout" srcset="http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/08/percent021.jpeg?w=400 400w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/08/percent021.jpeg?resize=225%2C300 225w" sizes="(max-width: 400px) 100vw, 400px" data-recalc-dims="1" />](http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/08/percent021.jpeg)

&nbsp;

프로젝트의 소스코드는 아래 주소에서 받을 수 있습니다.

  * <https://github.com/skyfe79/HelloPercentLayout>

PercentLayout의 자세한 내용은 공식 문서를 참고하세요.

  * <https://developer.android.com/reference/android/support/percent/PercentRelativeLayout.html>
  * <https://developer.android.com/reference/android/support/percent/PercentFrameLayout.html>