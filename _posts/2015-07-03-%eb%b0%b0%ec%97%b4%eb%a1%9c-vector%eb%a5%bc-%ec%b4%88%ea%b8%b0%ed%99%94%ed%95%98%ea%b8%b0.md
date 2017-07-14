---
id: 513
title: 배열로 vector를 초기화하기
date: 2015-07-03T16:39:20+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=513
permalink: '/%eb%b0%b0%ec%97%b4%eb%a1%9c-vector%eb%a5%bc-%ec%b4%88%ea%b8%b0%ed%99%94%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3900491727"
categories:
  - C++
tags:
  - vector
---
배열로 vector를 초기화 해보자.<!--more-->

<pre class="lang:default decode:true">$ vi printVector.cpp</pre>

<pre class="lang:default decode:true">#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;algorithm&gt;

void printVector(std::vector&lt;int&gt;& v);

int main() {

    int a[5] { 1, 2, 3, 4, 5 };

    std::vector&lt;int&gt; v;

    v.assign(a, a+5);


    printVector(v);
}


void printVector(std::vector&lt;int&gt;& v) {
    std::for_each(v.begin(), v.end(), [](int e) { std::cout &lt;&lt; e &lt;&lt; ", "; });
}
</pre>

<pre class="lang:default decode:true ">$ g++ printVector.cpp -std=c++11 -o printVector
./printVector
1, 2, 3, 4, 5,</pre>

&nbsp;