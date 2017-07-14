---
id: 326
title: 이벤트 기반 프로그래밍
date: 2015-03-14T21:18:54+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=326
permalink: '/%ec%9d%b4%eb%b2%a4%ed%8a%b8-%ea%b8%b0%eb%b0%98-%ed%94%84%eb%a1%9c%ea%b7%b8%eb%9e%98%eb%b0%8d/'
dsq_thread_id:
  - "3721654808"
categories:
  - Android
  - Cocoa
  - iOS
  - 컴퓨터이야기
tags:
  - event driven programming
---
이벤트 기반 프로그래밍은 프로그래밍 패러다임 중 하나로 프로그램의 흐름이 특정 이벤트에 따라 결정되는 것을 말한다.

<pre class="lang:default decode:true ">void main() {
    a();
    b();
    c();
}</pre>

<pre class="lang:default decode:true ">void main() {

    while(1) {
	
        int ch = getch();
        if(ch == 'a') a();
        if(ch == 'b') b();
        if(ch == 'c') c();
		
        if(ch == 'x') break;
    }
}</pre>

위 두 코드 중 아래의 코드가 이벤트 기반 프로그래밍이다. 첫번째것은 수행할 작업을 배치해 놓고 배치 순서대로 실행하고 프로그램을 종료한다. 아래의 코드는 무한 루프를 돌면서 사용자의 키입력을 기다리고 키입력에 따라 특정 작업을 수행한다. 두 코드의 가장 차이점은 무한 루프가 있다는 것이다. 이 무한 루프의 역할은 이벤트가 발생할 때까지 프로그램이 죽지 않게 살려두는 것이다. 이 루프를 메인 루프라고 할 때, 대부분의 이벤트 기반 프로그램들은 이러한 메인 루프를 가지고 있다. 메인 루프에서 이벤트를 기다리고 특정 이벤트가 발생했을 때 그 이벤트에 따른 콜백을 호출하는 식이다. 그래서 이벤트 기반 프로그램은 대부분 아래 그림과 같은 구조를 가지고 있다.

![](http://eventdrivenpgm.sourceforge.net/event_driven_programming.gif)
  
  <p class="wp-caption-text">
    출처 : http://eventdrivenpgm.sourceforge.net/
  </p>
</div>

위의 그림대로 위에서 작성한 코드를 다시 작성해 보면 아래와 같다.

<pre class="lang:default decode:true">void main() {

    bool exit = false;
    while(!exit) {
        
        int event = eventGenerator();
	exit = eventDispatcher(event);

    }
}

int eventGenerator() {
	return getch();
}

bool eventDispatcher(int keyEvent) {

	if(keyEvent == 'a') 
		handlerA();
		
	if(keyEvent == 'b') 
		handlerB();
		
	if(keyEvent == 'c') 
		handlerC();

	if(keyEvent == 'x') 
		return true;
	
	return false;
}
</pre>

이러한 이벤트 기반 프로그래밍은 GUI 프로그램과 웹프로그램 등에서 쓰이며 하드웨어에서는 인터럽트 처리 등에 쓰인다.

iOS, Android 등의 모바일 프로그래밍도 모두 이벤트 기반 프로그래밍이다. 따라서 각 프레임워크에서 이벤트를 기다리는 메인 루프가 무엇이며 이벤트의 디스패치(이벤트 전달)는 어떻게 이뤄지는지 아는 것은 필수다. iOS의 프레임워크인 Cocoa Touch 는 NSRunLoop 가 이벤트 수신을 기다리는 메인 루프를 담당하며 안드로이드에서는 메인스레드에서의 루퍼가 담당한다.

  * <http://eventdrivenpgm.sourceforge.net/>
  * [event\_driven\_programming](http://blog.burt.pe.kr/wp-content/uploads/2015/03/event_driven_programming.pdf)
  * <http://en.wikipedia.org/wiki/Event-driven_programming>