---
id: 235
title: Android Studio 코드 네비게이션
date: 2015-01-17T06:47:35+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=235
permalink: /android-studio-code-navigation/
dsq_thread_id:
  - "3724005038"
categories:
  - Android
tags:
  - android studio
  - code
  - navigation
---
코딩을 할 때 코드 네비게이션이 쉬워야 한다. 코드 네비게이션을 위해서 마우스를 사용해야 한다면 그것처럼 작업의 흐름을 끊는 것이 따로 없다. 이 글에서는 Android Studio 의 코드 네비게이션을 정리해 보려고 한다.<!--more-->

이 글은 Mac OS X 를 기준으로 하며 Android Studio의 keymap 을 Mac OS X 10.5+ 기준으로 설명한다.

[<img class="alignnone size-full wp-image-236" src="http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/01/스크린샷-2015-01-17-오후-3.14.29.png?resize=665%2C396" alt="스크린샷 2015-01-17 오후 3.14.29" srcset="http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/01/스크린샷-2015-01-17-오후-3.14.29.png?w=718 718w, http://i0.wp.com/burt.pe.kr/wp-content/uploads/2015/01/스크린샷-2015-01-17-오후-3.14.29.png?resize=300%2C179 300w" sizes="(max-width: 665px) 100vw, 665px" data-recalc-dims="1" />](http://i1.wp.com/blog.burt.pe.kr/wp-content/uploads/2015/01/스크린샷-2015-01-17-오후-3.14.29.png)

&nbsp;

##### 클래스 열기

> Cmd + o

MainActivity.java 파일로 이동하고 싶을 경우에 Cmd + o 를 누르고 MainA 정도까지만 치면 파일이름이 목록으로 나온다. 이름을 선택하면 해당 파일로 이동한다. 아주 빈번하게 사용되는 단축키이다.

##### 파일 열기

> Cmd + Shift + o

클래스 열기와 비슷하지만 리소스의  xml 파일 등을 열 때는 이 단축키를 사용해야 한다.

##### 심볼 열기

> Cmd + Alt + o

심볼을 찾아서 열 때 사용한다. 변수나 메서드의 이름을 입력하면 해당 변수나 메서드가 선언된 곳이로 이동할 수 있다.

##### 파일을 열어 특정 줄로 이동하기

Cmd + o 를 누른 다음 나오는 다이얼로그에서 ma:23 을 치면 MainActivity.java의 23번째 줄로 이동한다. (ma 로 시작하는 클래스가 MainActivity 만 있다고 가정)

##### 최종 편집 위치로 이동하기

> Cmd + Shift + backspace

정말 많이 사용하는 단축키! 최종 편집 위치로 이동한다. 파일 파일 내에서도 가능하다.

##### 코드 네비게이션 히스토리 뒤로 / 앞으로 이동하기

> Cmd + Alt + Left or Right

웹브라우져의 뒤로, 앞으로 이동하기 버튼처럼 코드 네비게이션 히스토리의 앞, 뒤로 움직인다.

##### 심볼이 사용된 곳 검색하기

> Alt + F7

Find 패널 내에서 심볼이 사용된 곳을 검색하여 보여준다.

> Cmd + Alt + F7

코드 편집기 내에서 심볼이 사용된 곳을 검색하여 보여준다.

##### 심볼이 선언된 곳으로 이동하기

> Cmd + b

##### 심볼이 구현된 곳으로 이동하기

> Cmd + Alt + b

##### 타입이 선언된 곳으로 이동하기

> Cmd + Shift + b

##### Super로 이동하기

> Cmd + u

<p class="table-caption">
  <p class="table-caption">
    아래 표는 구글 문서에서 발췌 ( 원문 : <a href="https://developer.android.com/sdk/installing/studio-tips.html">https://developer.android.com/sdk/installing/studio-tips.html</a> )
  </p>
  
  <p class="table-caption">
    <strong>Table 1.</strong> Programming key commands
  </p>
  
  <table>
    <tr>
      <th>
        Action
      </th>
      
      <th>
        Android Studio Key Command
      </th>
    </tr>
    
    <tr>
      <td>
        <strong>Command look-up (autocomplete command name)</strong>
      </td>
      
      <td>
        CMD + SHIFT + A
      </td>
    </tr>
    
    <tr>
      <td>
        Project quick fix
      </td>
      
      <td>
        ALT + ENTER
      </td>
    </tr>
    
    <tr>
      <td>
        Reformat code
      </td>
      
      <td>
        OPTION + CMD + L (Mac)
      </td>
    </tr>
    
    <tr>
      <td>
        Show docs for selected API
      </td>
      
      <td>
        F1 (Mac)
      </td>
    </tr>
    
    <tr>
      <td>
        Show parameters for selected method
      </td>
      
      <td>
        CTRL + P
      </td>
    </tr>
    
    <tr>
      <td>
        Generate method
      </td>
      
      <td>
        CMD + N (Mac)
      </td>
    </tr>
    
    <tr>
      <td>
        Jump to source
      </td>
      
      <td>
        CMD + down-arrow (Mac)
      </td>
    </tr>
    
    <tr>
      <td>
        Delete line
      </td>
      
      <td>
        CMD + Backspace (Mac)
      </td>
    </tr>
    
    <tr>
      <td>
        Search by symbol name
      </td>
      
      <td>
        OPTION + CMD + O (Mac)
      </td>
    </tr>
  </table>
  
  <p>
    &nbsp;
  </p>