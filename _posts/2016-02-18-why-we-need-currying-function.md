---
id: 613
title: 커링(currying) 함수가 왜 필요한가?
date: 2016-02-18T00:38:12+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=613
permalink: /why-we-need-currying-function
dsq_thread_id:
  - "4587490604"
categories:
  - Haskell
tags:
  - currying
  - haskell
---
 
명령형 언어에서 함수는 보통 0개에서 n개의 인자를 동시에 받아 특정 알고리즘을 수행한다.

<pre class="lang:c decode:true">int currentYear()
int nextYear(int currentYear)
int nextYearMonthDay(int currentYear, int currentMonth, int currentDay)</pre>

함수형 언어에서 함수는 보통 커링함수를 사용하는데 커링 함수는 n개의 인자를 받는 함수가 있을 경우 n개의 인자를 동시에 받는 것이 아니라 1개씩 인자를 받으며 다음 인자를 받는 함수를 반환하는 함수를 말한다.

int add( int a, int b ) 라는 함수가 있을 경우, 이를 커링함수로 구현하면 아래와 같다.

<pre class="lang:c decode:true ">add (int a) -&gt; (int -&gt; int) {
     return { b in
          return a + b
     }
}
</pre>
  
add(3) 을 호출하면 결과로 { return 3 + b } 의 람다 함수가 반환된다. 따라서 아래와 같이 호출할 수 있다.

<pre class="lang:c decode:true">add(3)(4)
     == 7
</pre>
  
**문제는 커링함수가 왜 필요한가? 이다.**

그것은 함수형 언어에서 데이터를 시퀀스(Sequence)라는 고차원의 리스트로 추상화해서 생각하는데 이 데이터 리스트에 어떤 연산을 가할 때 커링함수가 필요하기 때문이다.

예를 들어

<pre class="lang:haskell decode:true">map (add 1) [1,2,3,4,5]</pre>
  
하면 뒤에 나오는 데이터 리스트 각 항목에 1이 더해지는 것이다.

<pre class="lang:haskell decode:true">[2,3,4,5,6]</pre>
  
즉, 모든 함수를 커링하여 설계를 하면 데이터 리스트를 transforming 하는 연산자로 사용할 수 있기 때문에 커링함수가 함수형 프로그래밍에서 매우 중요해 지는 것이다.