---
id: 582
title: NDK 라이브러리 배포 시 java.lang.UnsatisfiedLinkError 에 대해서.
date: 2015-10-06T15:11:04+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=582
permalink: '/ndk-%eb%9d%bc%ec%9d%b4%eb%b8%8c%eb%9f%ac%eb%a6%ac-%eb%b0%b0%ed%8f%ac-%ec%8b%9c-java-lang-unsatisfiedlinkerror-%ec%97%90-%eb%8c%80%ed%95%b4%ec%84%9c/'
dsq_thread_id:
  - "4198356718"
categories:
  - Android
  - 프로젝트이야기
tags:
  - java.lang.UnsatisfiedLinkError
---
한 프로젝트에서 두 개 이상의 ndk 라이브러리를 사용한다고 가정해 봅시다. 각 라이브러리는 아래와 같이 ABI를 지원한다면 앱을 빌드하고 실행했을 때 잘 실행될까요?

NDK-Library A

  * armeabi

NDK-Library B

  * arm64-8a
  * armeabi
  * armeabi-v7a
  * mips
  * mips64
  * x86
  * x86_64

즉, NDK-Library B는 모든 ABI를 지원하고 NDK-Library A는 armeabi만 지원합니다. 이럴 경우 앱을 빌드하고 실행하면 NDK-Library A를 찾을 수 없다고 예외를 뿌리며 죽습니다. 즉, java.lang.UnsatisfiedLinkError 가 발생합니다.

이것을 해결하는 방법은 두 라이브러리가 지원 ABI를 동일하게 맞춰주는 수 밖에 없습니다. 즉, 이 경우에는 NDK-Library B를 수정해서 armeabi만 빌드해서 배포해야 합니다.  코드가 있을 경우에는 상관이 없지만 코드가 없을 경우에는 어떻게 해야할지&#8230;?

&nbsp;