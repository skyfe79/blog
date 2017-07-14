---
id: 8
title: 서브도메인 VirtualHost 설정하기
date: 2014-11-04T07:33:06+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=8
permalink: '/%ec%84%9c%eb%b8%8c%eb%8f%84%eb%a9%94%ec%9d%b8-virtualhost-%ec%84%a4%ec%a0%95%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "3708258199"
categories:
  - DigitalOcean
tags:
  - apache2
  - digitalocea
  - subdomain
  - vitualhost
---
1. 서브도메인에 연결할 디렉토리 생성하기

<pre class="lang:default decode:true">mkdir lab
sudo chown -R $USER:$USER lab</pre>

2. 서브도메인을 위한 아파치 VirtualHost, sites-available 설정하기

<pre class="lang:default decode:true">sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/lab.burt.pe.kr.conf
sudo vi /etc/apache2/sites-available/lab.burt.pe.kr.conf</pre>

아래처럼 VirtualHost 를 설정한다

<pre class="lang:default decode:true ">&lt;VirtualHost *:80&gt;
	ServerAdmin webmaster@localhost
	DocumentRoot /home/burt/www/lab
	ServerName lab.burt.pe.kr
	ServerAlias lab.burt.pe.kr
	 &lt;Directory /home/burt/www/lab&gt;
	     Options Indexes FollowSymLinks MultiViews
	     AllowOverride All
	     Order allow,deny
	     Allow from all
	 &lt;/Directory&gt;
     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
 &lt;/VirtualHost&gt;
</pre>

3. apache2.conf에서 퍼미션 설정하기

<pre class="lang:default decode:true ">sudo vi /etc/apache2/apache2.conf</pre>

아래처럼 작성한다

<pre class="lang:default decode:true ">&lt;Directory /home/burt/www/lab&gt;
	Options Indexes FollowSymLinks
	AllowOverride None
	Require all granted
&lt;/Directory&gt;</pre>

4. a2ensite 실행하기

<pre class="lang:default decode:true ">sudo a2ensite lab.burt.pe.kr</pre>

5. 아파치 재시작하기

<pre class="lang:default decode:true ">sudo service apache2 reload</pre>

6. 접속테스트 해 보기

만약 접속이 안된다면 digitalocean.com 에 접속해서 DNS를 설정한다.

[<img class="alignnone size-medium wp-image-12" src="http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2014/11/dns-300x93.png?resize=300%2C93" alt="dns" srcset="http://i2.wp.com/burt.pe.kr/wp-content/uploads/2014/11/dns.png?resize=300%2C93 300w, http://i2.wp.com/burt.pe.kr/wp-content/uploads/2014/11/dns.png?w=810 810w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" />](http://i0.wp.com/blog.burt.pe.kr/wp-content/uploads/2014/11/dns.png)