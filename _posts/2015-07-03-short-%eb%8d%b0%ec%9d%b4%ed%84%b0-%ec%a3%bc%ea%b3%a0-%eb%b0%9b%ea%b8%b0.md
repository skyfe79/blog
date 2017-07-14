---
id: 489
title: Short 데이터 주고 받기
date: 2015-07-03T13:39:42+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=489
permalink: '/short-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/'
dsq_thread_id:
  - "3998059871"
categories:
  - Android
  - C++
  - Java
tags:
  - jni
---
<p class="p1">
  <span class="s1">두 개의 값을 전달해 최대값을 반환하는 함수를 작성해 보자. </span><!--more-->
</p>

<pre class="lang:default decode:true ">$ vi PassAndReturnShort.cpp</pre>

<pre class="lang:default decode:true ">#include &lt;jni.h&gt;

jshort max
(
    JNIEnv      *env,
    jobject     thiz,
    jshort      a,
    jshort      b
)
{
    if( a &gt; b )
        return a;
    return b;
}

JNIEXPORT jint JNICALL JNI_OnLoad
(
    JavaVM      *vm,
    void        *reserved
)
{
    JNIEnv      *env;
    if(vm-&gt;GetEnv(reinterpret_cast&lt;void **&gt;(&env), JNI_VERSION_1_6)) {
        return -1;
    }
    
    JNINativeMethod nm {
        const_cast&lt;char *&gt;("max"),
        const_cast&lt;char *&gt;("(SS)S"),
        reinterpret_cast&lt;void *&gt;(max)
    };
    
    jclass cls = env-&gt;FindClass("Client");
    env-&gt;RegisterNatives(cls, &nm, 1);
    return JNI_VERSION_1_6;
}</pre>

컴파일하고 라이브러리로 만든다.

<pre class="lang:default decode:true ">$ g++ "-I/System/Library/Java/JavaVirtualMachines/Current/" -std=c++11 -c PassAndReturnShort.cpp
$ g++ -dynamiclib -o libparshort.jnilib passandreturnshort.o</pre>

<p class="p1">
  <span class="s1">라이브러리를 사용하는 자바 코드를 작성한다.</span>
</p>

<pre class="lang:default decode:true ">$ vi Client.java</pre>

<pre class="lang:default decode:true ">public class Client {

    public native short max(short a, short b);
    
    public static void main(String[] args) {
        
        Client client = new Client();
        
        System.out.println("choose the max value between 10, 20 : " + client.max((short)10, (short)20));
        System.out.println("choose the max value between 50, 10 : " + client.max((short)50, (short)10));
    }
    
    static {
        System.loadLibrary("parshort");
    }
}</pre>

<p class="p1">
  <span class="s1">자바 코드를 컴파일하고 실행한다.</span>
</p>

<pre class="lang:default decode:true ">$ javac Client.java
$ java Client
choose the max value between 10, 20 : 20
choose the max value between 50, 10 : 50</pre>

&nbsp;