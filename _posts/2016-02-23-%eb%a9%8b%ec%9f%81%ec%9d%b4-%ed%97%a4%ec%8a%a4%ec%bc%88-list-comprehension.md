---
id: 648
title: 멋쟁이 헤스켈, List Comprehension!
date: 2016-02-23T01:35:13+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=648
permalink: '/%eb%a9%8b%ec%9f%81%ec%9d%b4-%ed%97%a4%ec%8a%a4%ec%bc%88-list-comprehension/'
dsq_thread_id:
  - "4601613524"
categories:
  - Haskell
---
헤스켈을 공부하기 시작한 것이 얼마 되지 않았지만, 보면 볼 수록 매력덩어리이다. 헤스켈에 푹 빠져버린 첫 순간은 List Comprehension을 접하고 나서다.<!--more-->

예를들어, 아래의 문제를 푼다고 생각해 보자.

  * 아래 조건을 만족하는 직각 삼각형을 모두 찾으세요. 
      * 세 변의 길이는 모두 정수다.
      * 각 변의 길이는 10보다 작거나 같다
      * 삼각형의 둘레는 24이다.

이 문제를 한 줄로 표현해 풀 수 있다.

<pre class="lang:haskell decode:true ">[ (a,b,c) | c &lt;- [1..10], a &lt;- [1..c], b &lt;- [1..a], a^2 + b^2 == c^2, a+b+c == 24]</pre>

따봉!

그나저나 모노이드와 모나드는 참 어렵구나 ㅠ.ㅠ

  * 문제 출처 
      * [가장 쉬운 하스켈 책](http://www.yes24.com/24/Goods/12155304?Acode=101)