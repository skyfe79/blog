---
id: 313
title: Android Studio AAR 파일 만들기 (3/3)
date: 2015-03-13T14:23:22+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=313
permalink: '/android-studio-aar-%ed%8c%8c%ec%9d%bc-%eb%a7%8c%eb%93%a4%ea%b8%b0-33/'
dsq_thread_id:
  - "3708220897"
categories:
  - Android
tags:
  - aar
  - android studio
---
이전 글에서 앱모듈을 수정해서 aar 라이브러리를 만들어 보았는데 라이브러리 작성 후 액티비티 등에 올려 테스트할 방법이 마땅치 않았다. 좀 더 알아본 결과 안드로이드 스튜디오에서 서브 모듈을 추가하는 방식으로 aar 라이브러리와 테스트 앱을 동시에 개발할 수 있었다. 우선 라이브러리 개발을 위해 프로젝트를 만든다.

예를 들어 SayLib 라이브러리를 만든다고 한다면 테스트 앱은 kr.pe.burt.saylib 의 app 모듈로 만들고 그 서브 모듈로 kr.pe.burt.saylib.library 의 aar 모듈을 만드는 것이 편리하다.

<div id="attachment_314" style="width: 610px" class="wp-caption aligncenter">
  <a href="http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.04.png"><img class="wp-image-314" src="http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.04.png?resize=600%2C524" alt="SayLib 프로젝트를 만들고 새로운 서브 모듈을 추가한다." srcset="http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.04.png?w=558 558w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.04.png?resize=300%2C262 300w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>
  
  <p class="wp-caption-text">
    SayLib 프로젝트를 만들고 새로운 서브 모듈을 추가한다.
  </p>
</div>

&nbsp;

<div id="attachment_315" style="width: 610px" class="wp-caption aligncenter">
  <a href="http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.26.png"><img class="wp-image-315" src="http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.26.png?resize=600%2C445" alt="모듈 종류는 Android Library로 선택한다." srcset="http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.26.png?w=934 934w, http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.26.png?resize=300%2C222 300w, http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.26.png?resize=660%2C489 660w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>
  
  <p class="wp-caption-text">
    모듈 종류는 Android Library로 선택한다.
  </p>
</div>

&nbsp;

<div id="attachment_316" style="width: 610px" class="wp-caption aligncenter">
  <a href="http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.55.21.png"><img class="wp-image-316" src="http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.55.21.png?resize=600%2C361" alt="모듈명을 library로 한다. 물론 임의 지정이 가능하다." srcset="http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.55.21.png?w=1084 1084w, http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.55.21.png?resize=300%2C181 300w, http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.55.21.png?resize=1024%2C617 1024w, http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.55.21.png?resize=660%2C398 660w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>
  
  <p class="wp-caption-text">
    모듈명을 library로 한다. 물론 임의 지정이 가능하다.
  </p>
</div>

&nbsp;

<div id="attachment_317" style="width: 610px" class="wp-caption aligncenter">
  <a href="http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.45.png"><img class="wp-image-317" src="http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.45.png?resize=600%2C443" alt="액티비티는 없도록 한다. 있어도 상관 없다." srcset="http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.45.png?w=936 936w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.45.png?resize=300%2C221 300w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2015/03/스크린샷-2015-03-13-오후-1.52.45.png?resize=660%2C487 660w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" /></a>
  
  <p class="wp-caption-text">
    예제를 위해서 액티비티는 없도록 한다. 있어도 상관 없다.
  </p>
</div>

이제 library 모듈의 build.gradle 파일을 살펴 보자.

<pre class="lang:default decode:true ">apply plugin: 'com.android.library'

android {
    compileSdkVersion 21
    buildToolsVersion "21.1.2"

    defaultConfig {
        minSdkVersion 14
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
</pre>

플러그인이 알아서 잘 설정되어 있다. 이 library 모듈을 app 모듈에서 사용하려면 아래처럼  app 모듈의 build.gradle 을 아래처럼 설정한다.

<pre class="lang:default mark:25 decode:true">apply plugin: 'com.android.application'

android {
    compileSdkVersion 21
    buildToolsVersion "21.1.2"

    defaultConfig {
        applicationId "kr.pe.burt.android.saylib"
        minSdkVersion 14
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
    compile project(':library')
}
</pre>

이렇게 하면 app 모듈에서 library 모듈을 사용할 수 있다.

<pre class="lang:default decode:true ">public class MainActivity extends ActionBarActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Say say = new Say();
        String hello = say.hello(getResources());
        TextView sayTv = (TextView)findViewById(R.id.say);
        sayTv.setText(hello);
    }
}</pre>

library 모듈을 aar 파일로 패키징 하려면 터미널에서 아래처럼 입력한다. 물론  gradleW 파일 있는 곳에서 해야 한다.

<pre class="lang:default decode:true ">$ ./gradleW :library:aR</pre>

이 명령 실행 후에 app 모듈을 빌드할 때 혹시 아래와 같은 오류가 발생할 수 있다.

> Android Studio Gradle Already disposed Module

이럴 경우에는 아래의 명령을 실행 후 다시 빌드를 수행한다.

<pre class="lang:default decode:true ">$ ./gradleW clean</pre>

다른 프로젝트에서 사용하기

gradle의 compile 문의 규칙은 다음과 같다.

<pre class="lang:default decode:true ">compile 'group:name:version'
</pre>

group은 패키지명이다. 로컬 파일의 aar 을 쓸 때 주의할 점은 name 이 파일의 이름이라는 것이다. saylib.aar 로 파일이름을 정했다면 다른 프로젝트에서 사용할 때 아래처럼 의존성에 선언해 주어야 한다.

<pre class="lang:default mark:4 decode:true">dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:21.0.3'
    compile 'kr.pe.burt.saylib.library:saylib:1.0@aar'
}</pre>

예제의 전체 코드는 아래 주소에서 볼 수 있다.

  * <https://github.com/BurtK/AndroidStudioAAR>