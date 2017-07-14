---
id: 296
title: Android Studio AAR 파일 만들기 (1/3)
date: 2015-03-09T17:59:43+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=296
permalink: '/android-studio-aar-%ed%8c%8c%ec%9d%bc-%eb%a7%8c%eb%93%a4%ea%b8%b0/'
dsq_thread_id:
  - "3708028632"
categories:
  - Android
tags:
  - aar
---
Android Studio 용 라이브러리 패키지인 aar 을 만들고 사용해 보자

### Android Studio 에서 프로젝트를 생성한다.

[<img class=" size-full wp-image-297 aligncenter" src="http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.27.38.png?resize=665%2C538" alt="스크린샷 2015-03-09 오후 5.27.38" srcset="http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.27.38.png?w=705 705w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.27.38.png?resize=300%2C243 300w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.27.38.png?resize=660%2C534 660w" sizes="(max-width: 665px) 100vw, 665px" data-recalc-dims="1" />](http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.27.38.png)

### Activity는 없는 것으로 한다.(있어도 되지만 예제용 aar은 없는 것으로 한다)

[<img class=" size-full wp-image-298 aligncenter" src="http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.28.34.png?resize=602%2C867" alt="스크린샷 2015-03-09 오후 5.28.34" srcset="http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.28.34.png?w=602 602w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.28.34.png?resize=208%2C300 208w" sizes="(max-width: 602px) 100vw, 602px" data-recalc-dims="1" />](http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-09-오후-5.28.34.png)

### 코드를 작성한다. Say라는 클래스를 만들고 hello메서드를 정의한다. 이 메서드는 사용자 장치의 언어 설정에 따른 인사 문자열을 반환한다. 인사 문자열의 로컬라이제이션 데이터는 aar에 패키징 할 것이다. 

```
package kr.pe.burt.android.mylib;

import android.content.res.Resources;

/**
 * Created by burt on 15. 3. 9..
 */
public class Say {
    public String hello(Resources res) {
        return res.getString(R.string.hello);
    }
}
```

```    
<resources>
     <string name="hello">Hello</string>
</resources>
    
<resources>
    <string name="hello">안녕하세요</string>
</resources>
```

### build.gradle 설정하기 

  1. 플러그인을 com.android.library 로 변경한다
  2. defaultConfig의 applicationId 를 삭제한다. 

```
apply plugin: 'com.android.library'

android {
    compileSdkVersion 21
    buildToolsVersion "21.1.2"

    defaultConfig {
        applicationId "kr.pe.burt.android.mylib"vmf
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
}
```

### aar 파일 만들기
  
    터미널을 열어서 프로젝트의 gradlew이  있는 곳으로 이동한다. 안드로이드 스튜디오에는 터미널을 쉽게 열 수 있는 터미널 창이 있으니 해당 창을 이용한다. 아래와 같이 빌드한다. 

```
./gradlew clean
./gradleW aR
```
    
위 명령줄에서 aR는 assembleRelease의 약자이다.

**결과물 aar 파일의 위치**

```
your-library-project
|- app
    |- build
        |- outputs
            |- aar
                |- app-release.aar
```
        
에 존재한다.