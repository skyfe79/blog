---
id: 453
title: Boolean 형 데이터 값 주고 받기
date: 2015-05-25T12:54:41+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=453
permalink: '/boolean-%ed%98%95-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ea%b0%92-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/'
dsq_thread_id:
  - "3791119557"
categories:
  - Android
  - C++
  - Java
tags:
  - jni
---
<p class="p1">
  <span class="s1">Boolean 데이터를 주고 받는 예제를 작성한다. </span><!--more-->
</p>

<pre class="lang:default decode:true ">$ vi PassAndReturnBool.cpp</pre>

<pre class="lang:default decode:true ">#include &lt;jni.h&gt;

jboolean toggle
(
	JNIEnv		*env,
	jobject		thiz,
	jboolean		value
)
{
	return !value;
}

JNIEXPORT jint JNICALL JNI_OnLoad
(
	JavaVM		*vm,
	void		*reserved
)
{
	JNIEnv *env;
	if(vm-&gt;GetEnv(reinterpret_cast&lt;void**&gt;(&env), JNI_VERSION_1_6)) {
		return -1;
	}
	
	JNINativeMethod	nm[1];
	nm[0].name = const_cast&lt;char *&gt;("toggle");
	nm[0].signature = const_cast&lt;char *&gt;("(Z)Z"); // boolean을 받고 boolean을 반환하는 함수 시그니쳐
	nm[0].fnPtr = reinterpret_cast&lt;void *&gt;(toggle);
	
	jclass cls = env-&gt;FindClass("Client");
	env-&gt;RegisterNatives(cls, nm, 1);
	return JNI_VERSION_1_6;
}</pre>

위의 코드에서 함수 시그니쳐에 대한 내용은 [&#8220;메서드등록&#8221;](http://blog.burt.pe.kr/%eb%a9%94%ec%84%9c%eb%93%9c-%eb%93%b1%eb%a1%9d/) 글을 참고한다.  <span class="s1">컴파일해서 libparbool.jnilib 파일을 만든다.</span>

<pre class="lang:default decode:true ">$ g++ "-I/System/Library/Frameworks/JavaVM.framework/Versions/Current/Headers/" -c PassAndReturnBool.cpp
$ g++ -dynamiclib -o libparbool.jnilib passandreturnbool.o</pre>

<p class="p1">
  <span class="s1">위 네이티브 라이브러리를 사용하는 Java클래스를 작성한다.</span>
</p>

<pre class="lang:default decode:true ">$ vi Client.java</pre>

<pre class="lang:default decode:true ">public class Client {

    private native boolean toggle(boolean value);

    public static void main(String[] args) {
        
        Client client = new Client();

        System.out.println("toggle(true) is " + client.toggle(true));
        System.out.println("toggle(false) is " + client.toggle(false));
    }

    static {
        System.loadLibrary("parbool");
    }
}</pre>

<p class="p1">
  <span class="s1">Java코드를 컴파일하고 실행한다.</span>
</p>

<pre class="lang:default decode:true ">$ javac Client.java
$ java Client
toggle(true) is false
toggle(false) is true</pre>

&nbsp;