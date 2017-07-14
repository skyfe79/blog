---
id: 568
title: TypedArray의 getDrawable()이 null을 반환한다면?
date: 2015-08-31T15:26:42+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=568
permalink: '/typedarray%ec%9d%98-getdrawable%ec%9d%b4-null%ec%9d%84-%eb%b0%98%ed%99%98%ed%95%9c%eb%8b%a4%eb%a9%b4/'
dsq_thread_id:
  - "4083492103"
categories:
  - Android
---
<pre class="lang:xhtml decode:true">&lt;declare-styleable name="WatermarkButton"&gt;
    &lt;attr name="watermarkImage" format="reference" /&gt;
&lt;/declare-styleable&gt;</pre>

위처럼 속성을 선언하고 아래 코드처럼 접근할 때, null을 반환한다면

<pre class="lang:java mark:1 decode:true">TypedArray typedArray = context.obtainStyledAttributes(attrs, R.styleable.WatermarkButton);
watermarkDrawable = typedArray.getDrawable(R.styleable.WatermarkButton_watermarkImage);
typedArray.recycle();
</pre>

해당 커스텀뷰를 선언한 xml로 이동하여 xml 네임스페이스를 제대로 적었는지 확인한다.

<pre class="lang:default decode:true ">xmlns:watermark="http://schemas.android.com/apk/res-auto"</pre>

이렇게 적어야 하는데 가끔 인텔리센스에 의해서

<pre class="lang:default decode:true ">xmlns:watermark="http://schemas.android.com/tools"</pre>

로 쓰여질 경우가 있다. 길고긴 삽질;