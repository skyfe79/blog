---
id: 444
title: Int 데이터 값 주고 받기
date: 2015-05-24T14:32:40+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=444
permalink: '/int-%eb%8d%b0%ec%9d%b4%ed%84%b0-%ea%b0%92-%ec%a3%bc%ea%b3%a0-%eb%b0%9b%ea%b8%b0/'
dsq_thread_id:
  - "3789164517"
categories:
  - Android
  - C++
  - Java
tags:
  - jni
---
<p class="p1">
  <span class="s1">앞으로 몇 개의 튜토리얼을 통해서 데이터를 자바 레이어에서 네이티브 레이어로 전달하고 어떤 연산을 거친 후 되돌려 받는 예제를 작성할 것이다. 우선 Int 형을 주고 받는 예제를 작성해 보자.</span><!--more-->
</p>

<p class="p1">
  <span class="s1">PassAndReturnInt.cpp 파일을 생성하고 작성한다.</span>
</p>

<pre class="lang:default decode:true  ">$ vi PassAndReturnInt.cpp</pre>

<pre class="lang:default decode:true ">#include &lt;jni.h&gt;

jint PassAndReturnInt
(
	JNIEnv		*env,
	jobject		 thiz,
	jint			 data
)
{
	return data * data;
}

JNIEXPORT jint JNICALL JNI_OnLoad
(
	JavaVM		*vm,
	void		*reserved
)
{
	JNIEnv *env;
	if(vm-&gt;GetEnv(reinterpret_cast&lt;void **&gt;(&env), JNI_VERSION_1_6)) {
		return -1;
	}
	
	JNINativeMethod nm[1];
	nm[0].name = const_cast&lt;char *&gt;("PassAndReturnInt");
	nm[0].signature = const_cast&lt;char *&gt;("(I)I");
	nm[0].fnPtr = reinterpret_cast&lt;void *&gt;(PassAndReturnInt);
	
	jclass cls = env-&gt;FindClass("Client");
	env-&gt;RegisterNatives(cls, nm, 1);
	return JNI_VERSION_1_6;
}</pre>

<p class="p1">
  <span class="s1">이제 이 코드를 libpassandreturnint.jnilib으로 만든다.</span>
</p>

<pre class="lang:default decode:true ">$ g++ "-I/System/Library/Frameworks/JavaVM.framework/Versions/Current/Headers/" -c PassAndReturnInt.cpp
$ g++ -dynamiclib -o libpassandreturnint.jnilib passandreturnint.o</pre>

이제 이 네이티브 라이브러리를 사용하는 Java클래스를 작성한다.

<pre class="lang:default decode:true ">$ vi Client.java</pre>

<pre class="lang:default decode:true ">public class Client {
    private native int PassAndReturnInt(int data);

    public static void main(String[] args) {
        
        Client c = new Client();
        System.out.println("pass 10 and return = " + c.PassAndReturnInt(10));
    }

    static {
        System.loadLibrary("passandreturnint");
    }
}</pre>

<p class="p1">
  <span class="s1">위의 Java코드를 컴파일하고 실행해 보자</span>
</p>

<pre class="lang:default decode:true ">$ javac Client.java
$ java Client
pass 10 and return = 100</pre>

&nbsp;