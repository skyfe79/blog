---
id: 27
title: Yosemite 에서 IntelliJ 13 CE, 실행 Crash 수정하기
date: 2014-11-04T10:08:49+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=27
permalink: '/yosemite-%ec%97%90%ec%84%9c-intellij-13-ce-%ec%8b%a4%ed%96%89-crash-%ec%88%98%ec%a0%95%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3730447933"
categories:
  - Java
tags:
  - crash
  - intellij
  - yosemite
---
1. IntelliJ 의 설정 파일의 JVM 버전과 Yosemite에 설치된 java의 버전이 맞지 않아서 생기는 문제다.

  1. 응용프로그램으로 이동한다
  2. IntelliJ IDEA 13 CE를 마우스로 선택한다
  3. 마우스 오른쪽 버튼을 눌러 컨텍스트 메뉴가 나오게 한다
  4. &#8216;패키지 내용 보기&#8217; 메뉴를 실행한다
  5. 다음 파일을 연다

<!--more-->2. 다음 파일을 연다

<pre class="lang:default decode:true">Contents &gt; Info.plist</pre>

3. Yosemite에 설치된 자바의 버전을 확인한다

<pre class="lang:default decode:true ">$ java -version

java version "1.7.0_65"</pre>

4. JVMVersion 항목을 위의 Java 버전으로 설정한다.

<pre class="lang:default decode:true ">&lt;key&gt;JVMVersion&lt;/key&gt;
&lt;string&gt;1.6*&lt;/string&gt;</pre>

을 아래처럼 수정한다.

<pre class="lang:xhtml decode:true">&lt;key&gt;JVMVersion&lt;/key&gt;
&lt;string&gt;1.7*&lt;/string&gt;</pre>

&nbsp;

&nbsp;

&nbsp;