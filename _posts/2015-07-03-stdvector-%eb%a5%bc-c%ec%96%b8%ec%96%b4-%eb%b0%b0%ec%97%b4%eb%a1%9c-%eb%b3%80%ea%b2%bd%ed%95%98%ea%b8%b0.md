---
id: 510
title: std::vector 를 C언어 배열로 변경하기
date: 2015-07-03T15:47:04+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=510
permalink: '/stdvector-%eb%a5%bc-c%ec%96%b8%ec%96%b4-%eb%b0%b0%ec%97%b4%eb%a1%9c-%eb%b3%80%ea%b2%bd%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3900383958"
categories:
  - C++
tags:
  - vector
---
vector<int> v 가 있을 때, v를 배열로 변경하는 방법은 아래와 같다.<!--more-->

<pre class="lang:default decode:true ">//C++11 이전
const int* vp = &v[0];

//C++11 이후
cosnt int* vp = v.data();</pre>

예제로 확인해 보자.

<pre class="lang:default decode:true ">$ vi printVector.cpp</pre>

<pre class="lang:default decode:true ">#include &lt;vector&gt;
#include &lt;iostream&gt;

using namespace std;

void printArray(const int*, int);

int main() {

    vector&lt;int&gt; v = { 1, 2, 3, 4, 5 };



    int *vp = &v[0];

    printArray(vp, 5);

    int *vpp = v.data();
    printArray(vpp, 5);

    return 0;
}


void printArray(const int* array, int size) {
    cout &lt;&lt; "vp : ";
    for(int i=0; i&lt;size; i++) {
        cout &lt;&lt; array[i] &lt;&lt; ", ";
    }
    cout &lt;&lt; endl;
}</pre>

컴파일 하고 실행해 보자.

<pre class="lang:default decode:true ">$ g++ printArray.cpp -std=c++11 -o printArray
./printArray
1, 2, 3, 4, 5,
1, 2, 3, 4, 5,</pre>

&nbsp;