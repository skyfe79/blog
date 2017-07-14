---
id: 492
title: char 데이터 주고 받기
date: 2015-07-03T13:44:01+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=492
permalink: '/char-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/'
dsq_thread_id:
  - "3900199631"
categories:
  - Android
  - C++
  - Java
tags:
  - jni
---
<p class="p1">
  <span class="s1">문자를 전달하면 다음 문자를 반환하는 함수를 작성해 보자.</span><!--more-->
</p>

<pre class="lang:default decode:true ">$ vi PassAndReturnChar.cpp</pre>

<pre class="lang:default decode:true ">#include &lt;jni.h&gt;

jchar nextChar
(
    JNIEnv      *env,
    jobject     thiz,
    jchar       ch
)
{
    return ch + 1;
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
        const_cast&lt;char*&gt;("nextChar"),
        const_cast&lt;char*&gt;("(C)C"),
        reinterpret_cast&lt;void*&gt;(nextChar)
    };
    
    jclass cls = env-&gt;FindClass("Client");
    env-&gt;RegisterNatives(cls, &nm, 1);
    return JNI_VERSION_1_6;
}</pre>

<p class="p1">
  <span class="s1">컴파일하고 라이브러리로 만든다.</span>
</p>

<pre class="lang:default decode:true ">$ g++ "-I/System/Library/Java/JavaVirtualMachines/Current/" -std=c++11 -c PassAndReturnChar.cpp
$ g++ -dynamiclib -o libparshort.jnilib passandreturnshort.o</pre>

<p class="p1">
  <span class="s1">라이브러리를 사용하는 자바 클래스를 만든다.</span>
</p>

<pre class="lang:default decode:true ">$ vi Client.java</pre>

<pre class="lang:default decode:true ">public class Client {

    public native char nextChar(char ch);
    
    public static void main(String[] args) {
        
        Client client = new Client();
        
        System.out.println("The next character of 'a' is " + client.nextChar('a'));
        System.out.println("The next character of 'B' is " + client.nextChar('B'));
    }
    
    static {
        System.loadLibrary("parchar");
    }
}</pre>

<p class="p1">
  <span class="s1">자바 코드를 컴파일하고 실행해 본다.</span>
</p>

<pre class="lang:default decode:true ">$ javac Client.java
$ java Client
The next character of 'a' is b
The next character of 'B' is C</pre>

&nbsp;