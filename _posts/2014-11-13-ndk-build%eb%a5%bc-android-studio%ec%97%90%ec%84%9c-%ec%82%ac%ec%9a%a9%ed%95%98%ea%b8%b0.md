---
id: 151
title: ndk-build를 Android Studio에서 사용하기
date: 2014-11-13T08:53:36+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=151
permalink: '/ndk-build%eb%a5%bc-android-studio%ec%97%90%ec%84%9c-%ec%82%ac%ec%9a%a9%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3708005525"
categories:
  - Android
---
[javah를 Android Studio에서 사용하기](http://blog.burt.pe.kr/?p=142) 를 참고해서 같은 방식으로 ndk-build를 외부 도구로 등록해서 사용하면 된다.<!--more-->

[<img class="size-full wp-image-153 aligncenter" src="http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-13-오후-5.52.13.png?resize=665%2C476" alt="" srcset="http://i0.wp.com/burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-13-오후-5.52.13.png?w=673 673w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-13-오후-5.52.13.png?resize=300%2C214 300w" sizes="(max-width: 665px) 100vw, 665px" data-recalc-dims="1" />](http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2014/11/스크린샷-2014-11-13-오후-5.52.13.png)

<pre class="lang:default decode:true ">program:
ndk-build

Working directory:
$SourcepathEntry$/../jni</pre>

주의할 점은 ndk-build에서 출력 디렉토리를 설정할 수가 없기 때문에 결과 파일이 src->main->libs 에 생긴다는 점이다. 만들어지 so 파일을 apk 에 담으려면  gradle 파일을 설정하면된다. 아래처럼 적어준다.

<pre class="lang:default decode:true ">android {
    ...
    sourceSets.main {
        jniLibs.srcDir 'src/main/libs'
        jni.srcDirs = [] //important!!!! disable automatic ndk-build call
    }
    ...
}</pre>

전체 gradle 파일 내용은 아래와 같다.

<pre class="lang:default decode:true  ">apply plugin: 'com.android.application'

android {
    compileSdkVersion 21
    buildToolsVersion "20.0.0"

    defaultConfig {
        applicationId "kr.pe.burt.android.hellondk"
        minSdkVersion 14
        targetSdkVersion 21
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            runProguard false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }

    sourceSets.main {
        //
        jniLibs.srcDir 'src/main/libs'
        //jni.srcDirs = [] //important!!!! disable automatic ndk-build call
    }

}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
}</pre>

&nbsp;