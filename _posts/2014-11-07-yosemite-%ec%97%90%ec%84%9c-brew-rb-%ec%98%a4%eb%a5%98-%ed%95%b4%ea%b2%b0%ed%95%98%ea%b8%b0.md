---
id: 72
title: Yosemite 에서 brew.rb 오류 해결하기
date: 2014-11-07T06:26:17+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=72
permalink: '/yosemite-%ec%97%90%ec%84%9c-brew-rb-%ec%98%a4%eb%a5%98-%ed%95%b4%ea%b2%b0%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3894079952"
categories:
  - brew.rb
  - Mac OS X
---
맥을 Yosemite 로 업그레이드를 한 후 커맨드를 실행할 때 brew.rb 와 관련해서 아래와 같은 오류가 발생할 때가 있다.<!--more-->

<pre class="lang:default decode:true ">/usr/local/bin/brew: /usr/local/Library/brew.rb: /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/bin/ruby: bad interpreter: No such file or directory
/usr/local/bin/brew: line 26: /usr/local/Library/brew.rb: Undefined error: 0</pre>

이것을 해결하려 할 때 brew update로는 해결 되지 않는다. 아래처럼 git pull 을 통해서 통째로 업그레이드 해야 한다.

<pre class="lang:default decode:true ">cd /usr/local/Library
git pull origin master</pre>

&nbsp;