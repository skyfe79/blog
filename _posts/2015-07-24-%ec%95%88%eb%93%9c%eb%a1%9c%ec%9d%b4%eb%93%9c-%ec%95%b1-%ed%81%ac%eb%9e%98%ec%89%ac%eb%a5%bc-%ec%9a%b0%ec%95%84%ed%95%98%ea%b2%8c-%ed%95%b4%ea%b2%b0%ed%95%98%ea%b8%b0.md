---
id: 527
title: 안드로이드 앱 크래쉬를 우아하게 해결하기
date: 2015-07-24T12:38:11+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=527
permalink: '/%ec%95%88%eb%93%9c%eb%a1%9c%ec%9d%b4%eb%93%9c-%ec%95%b1-%ed%81%ac%eb%9e%98%ec%89%ac%eb%a5%bc-%ec%9a%b0%ec%95%84%ed%95%98%ea%b2%8c-%ed%95%b4%ea%b2%b0%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3965045891"
categories:
  - Android
tags:
  - android
  - app
  - crash
  - 앱 크래쉬
---
안드로이드 앱 크래쉬가 발생할 경우, 앱이 죽었다는 메시지박스를 화면에 보여주는데 사용자에게 불쾌감을 줄 정도로 예쁘지 않죠. 며칠전 오픈소스로 [CustomActivityOnCrash](https://github.com/Ereza/CustomActivityOnCrash) 라는 프로젝트가 오픈되었는데요. [CustomActivityOnCrash](https://github.com/Ereza/CustomActivityOnCrash)를 사용하면 앱 크래쉬가 발생할 경우 커스텀 액티비티를 화면에 띄울 수 있어 아주 우아하게 앱 크래쉬를 해결할 수 있습니다.

<!--more-->

[<img class=" size-full wp-image-528 aligncenter" src="http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/07/frontpage.png?resize=665%2C473" alt="CustomActivityOnCrash" srcset="http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/07/frontpage.png?w=675 675w, http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/07/frontpage.png?resize=300%2C213 300w, http://i1.wp.com/burt.pe.kr/wp-content/uploads/2015/07/frontpage.png?resize=660%2C469 660w" sizes="(max-width: 665px) 100vw, 665px" data-recalc-dims="1" />](http://i2.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/07/frontpage.png)

&nbsp;

사용하는 방법도 아주 간편합니다. 우선 build.gradle 에 디펜던시를 추가해 줍니다.

<pre class="lang:default decode:true ">dependencies {
    compile 'cat.ereza:customactivityoncrash:1.1.0'
}</pre>

그리고 Application을 상속받은 커스텀 Application 클래스를 만들고 CustomActivityOnCrash 를 설정해 주면 됩니다.

<pre class="lang:default decode:true">public class MyApp extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        //CustomActivityOnCrash 설치
        CustomActivityOnCrash.install(this);
    }
}</pre>

CustomActivityOnCrash 에는 앱 크래쉬가 발생할 경우 실행할 기본 액티비티가 있습니다. 에러 로그등을 볼 수 있어 개발할 경우에도 많이 편할 것 같네요. 만약 이 기본 액티비티를 다른 것으로 바꾸고자 할 경우에는 아래처럼 설정해 주면 됩니다.

<pre class="lang:default decode:true ">public class MyApp extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        //CustomActivityOnCrash 설치
        CustomActivityOnCrash.setErrorActivityClass(MyErrorActivity.class)
        CustomActivityOnCrash.install(this);
    }
}</pre>

[CustomActivityOnCrash 의 문서](https://github.com/Ereza/CustomActivityOnCrash)를 보면 이 밖에도 여러가지 설정을 할 수 있습니다.