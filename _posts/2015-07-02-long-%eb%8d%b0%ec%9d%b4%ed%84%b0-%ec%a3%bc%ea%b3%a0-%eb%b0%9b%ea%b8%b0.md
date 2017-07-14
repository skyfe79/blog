---
id: 480
title: Long 데이터 주고 받기
date: 2015-07-02T17:03:12+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=480
permalink: '/long-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/'
dsq_thread_id:
  - "3897288131"
categories:
  - C++
  - Java
tags:
  - jni
---
<p class="p1">
  <span class="s1">숫자를 전달하면 전달한 숫자의 팩토리얼을 반환한는 예제를 작성해 보자. 팩토리얼 함수를 네이티브로 작성한다.</span><!--more-->
</p>

<pre class="lang:default decode:true ">$ vi PassAndReturnLong.cpp</pre>

<pre class="lang:default decode:true ">#include &lt;jni.h&gt;

jlong factorial
(
    JNIEnv      *env,
    jobject     thiz,
    jlong       num
)
{
    if(num &lt;= 1) return 1L;
    
    return num * factorial(env, thiz, num-1);
}


JNIEXPORT jint JNICALL JNI_OnLoad
(
    JavaVM      *vm,
    void        *reserved
)
{
    JNIEnv      *env;
    if(vm-&gt;GetEnv(reinterpret_cast&lt;void**&gt;(&env), JNI_VERSION_1_6)) {
        return -1;
    }
    
    JNINativeMethod nm {
        const_cast&lt;char *&gt;("factorial"),
        const_cast&lt;char *&gt;("(J)J"),
        reinterpret_cast&lt;void *&gt;(factorial)
    };
    
    jclass cls = env-&gt;FindClass("Client");
    env-&gt;RegisterNatives(cls, &nm, 1);
    return JNI_VERSION_1_6;
}</pre>

<p class="p1">
  <span class="s1">재귀함수로 팩토리얼을 구현했다. 컴파일하고 라이브러리를 만든다.</span>
</p>

<pre class="lang:default decode:true ">$ g++ "-I/System/Library/Frameworks/JavaVM.framework/Versions/Current/Headers/" -std=c++11 -c PassAndReturnLong.cpp
$ g++ -dynamiclib -o libparlong.jnilib passandreturnlong.o</pre>

<p class="p1">
  <span class="s1">라이브러리를 사용하는 자바 코드를 작성한다.</span>
</p>

<pre class="lang:default decode:true ">$ vi Client.java</pre>

<pre class="lang:default decode:true ">public class Client {

    public native long factorial(long num);
    
    public static void main(String[] args) {
        
        Client client = new Client();
        
        System.out.println("The factorial of 10 is " + client.factorial(10));
        System.out.println("The factorial of 15 is " + client.factorial(15));
    }
    
    static {
        System.loadLibrary("parlong");
    }
}</pre>

<p class="p1">
  <span class="s1">컴파일하고 실행해 본다.</span>
</p>

<pre class="lang:default decode:true ">$ javac Client.java
$ java Client
The factorial of 10 is 3628800
The factorial of 15 is 1307674368000</pre>

&nbsp;