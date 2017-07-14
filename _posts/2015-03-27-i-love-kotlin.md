---
id: 354
title: I love Kotlin!
date: 2015-03-27T15:09:42+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=354
permalink: /i-love-kotlin/
dsq_thread_id:
  - "3708156622"
categories:
  - Android
  - Kotlin
tags:
  - kotlin
---
안드로이드 개발을 위해서 Kotlin 을 적극 사용하기로 했다. Scaloid를 이용해서 scala 기반으로 개발할 수 있겠으나 scala의 라이브러리 크기가 커서 일단 보류하였다. Kotlin을 선택한 이유는 아래와 같다.

  * **Relatively fast learning curve**: compared to Scala for instance, we are moving in a much simpler scope. Kotlin is much more limited, but it´s easier to start if you´ve never used a modern language before.
  * **Lightweight**: Kotlin library is small compared to others. This is important because Android method limit is always a problem, and though there are some options to solve it such as proguard or multidexing, all of these solutions will add complexity and will be time consuming when debugging. Kotlin adds less than 7000 methods, more or less the same as support-v4.
  * **Highly interoperable**: It works extremely well with any other Java libraries, and the interoperability is very simple. That´s one of the main ideas the Kotlin team kept in mind while developing this new language. They want to use it to continue developing their current projects written in Java without having to rewrite the whole code. So Kotlin needs to be extremely interoperable with Java code.
  * **Perfectly integrated with Android Studio and Gradle**: we have one plugin for the IDE and another one for Gradle, so it won´t be difficult to start an Android project using Kotlin (this is what I´ll talk about in the next article).
  * **Support Native Method Declaration**: You can use C/C++ modules through the JNI.

출처 : <http://antonioleiva.com/kotlin-for-android-introduction/>