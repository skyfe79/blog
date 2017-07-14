---
id: 302
title: Android Studio AAR 파일 만들기 (2/3)
date: 2015-03-09T18:19:06+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=302
permalink: '/android-studio-aar-%ed%8c%8c%ec%9d%bc-%eb%a7%8c%eb%93%a4%ea%b8%b0-22/'
dsq_thread_id:
  - "3715304619"
categories:
  - Android
tags:
  - aar
---
만들어진 aar 파일을 사용해 보자.<!--more-->

### 프로젝트를 새로 생성하고 텍스트뷰가 있는 액티비티를 만든다. 

<pre class="lang:default decode:true" title="activity_main.xml">&lt;RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity"&gt;

    &lt;TextView
        android:id="@+id/tvHello"
        android:text="@string/hello_world"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" /&gt;

&lt;/RelativeLayout&gt;
</pre>
    
```
package kr.pe.burt.android.testmylib;

import android.os.Bundle;
import android.support.v7.app.ActionBarActivity;
import android.widget.TextView;

import kr.pe.burt.android.mylib.Say;


public class MainActivity extends ActionBarActivity {

    TextView tvHello;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvHello = (TextView)findViewById(R.id.tvHello);

        Say say = new Say();
        tvHello.setText(say.hello(getResources()));
    }
}
```
    
    
### mylib.aar 파일을 libs 폴더에 복사한다.

[<img class=" fit-width size-full wp-image-303 aligncenter" src="http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-6.13.02.png?resize=602%2C386" alt="스크린샷 2015-03-09 오후 6.13.02" srcset="http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-6.13.02.png?w=602 602w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-6.13.02.png?resize=300%2C192 300w" sizes="(max-width: 602px) 100vw, 602px" data-recalc-dims="1" />](http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-6.13.02.png)

### build.gradle 파일을 작성한다. 

```
...
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs' //이렇게 해야 libs 폴더에서 aar 파일을 찾을 수 있다 
    }
}

...


dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:21.0.3'
    compile 'kr.pe.burt.android.mylib:mylib:1.0@aar'
}

```
        
아래는 build.gradle 파일의 전문이다.
        
```
apply plugin: 'com.android.application'

repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
}

android {
    compileSdkVersion 21
    buildToolsVersion "21.1.2"

    defaultConfig {
        applicationId "kr.pe.burt.android.testmylib"
        minSdkVersion 15
        targetSdkVersion 21
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
    compile 'com.android.support:appcompat-v7:21.0.3'
    compile 'kr.pe.burt.android.mylib:mylib:1.0@aar'
}
```
    
### 결과화면
    
<img class="  wp-image-304 aligncenter" src="http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/KakaoTalk_Photo_2015-03-09-18-17-10.jpeg?resize=337%2C449" alt="KakaoTalk_Photo_2015-03-09-18-17-10" srcset="http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/03/KakaoTalk_Photo_2015-03-09-18-17-10.jpeg?w=720 720w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/03/KakaoTalk_Photo_2015-03-09-18-17-10.jpeg?resize=225%2C300 225w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/03/KakaoTalk_Photo_2015-03-09-18-17-10.jpeg?resize=660%2C880 660w" sizes="(max-width: 337px) 100vw, 337px" data-recalc-dims="1" />
