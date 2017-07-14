---
id: 496
title: byte 데이터 주고 받기
date: 2015-07-03T13:46:53+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=496
permalink: '/byte-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/'
dsq_thread_id:
  - "3900170976"
categories:
  - Android
  - C++
  - Java
tags:
  - jni
---
<p class="p1">
  <span class="s1">숫자를 전달하면 2의 보수를 반환하는 함수를 작성한다.</span><!--more-->
</p>

<pre class="lang:default decode:true">$ vi PassAndReturnByte.cpp</pre>

<pre class="lang:default decode:true ">#include &lt;jni.h&gt;

jbyte complement2
(
    JNIEnv      *env,
    jobject     thiz,
    jbyte       value
)
{
    return (~value) + 1;
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
        const_cast&lt;char*&gt;("complement2"),
        const_cast&lt;char*&gt;("(B)B"),
        reinterpret_cast&lt;void*&gt;(complement2)
    };
    
    jclass cls = env-&gt;FindClass("Client");
    env-&gt;RegisterNatives(cls, &nm, 1);
    return JNI_VERSION_1_6;
}</pre>

<p class="p1">
  <span class="s1">컴파일하고 라이브러리로 만든다.</span>
</p>

<pre class="lang:default decode:true ">$ g++ "-I/System/Library/Java/JavaVirtualMachines/Current/" -std=c++11 -c PassAndReturnByte.cpp
$ g++ -dynamiclib -o libparbyte.jnilib passandreturnbyte.o</pre>

<p class="p1">
  <span class="s1">라이브러리를 사용하는 자바 코드를 작성한다.</span>
</p>

<pre class="lang:default decode:true ">$ vi Client.java</pre>

<pre class="lang:default decode:true ">public class Client {

    public native byte complement2(byte value);
    
    public static void main(String[] args) {
        
        Client client = new Client();
        
        System.out.println("The two's complement of 2 is " + client.complement2((byte)2));
        System.out.println("The two's complement of 3 is " + client.complement2((byte)3));
        System.out.println("The two's complement of -1 is " + client.complement2((byte)-1));
    }
    
    static {
        System.loadLibrary("parbyte");
    }
}</pre>

<p class="p1">
  <span class="s1">컴파일하고 실행해 본다.</span>
</p>

<pre class="lang:default decode:true ">$ javac Client.java
$ java Client
The two's complement of 2 is -2
The two's complement of 3 is -3
The two's complement of -1 is 1</pre>

&nbsp;