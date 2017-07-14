---
id: 471
title: C/C++ 에서 GLSL 코드를 쉽게 포함해 보자.
date: 2015-06-19T17:42:21+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=471
permalink: '/cc-%ec%97%90%ec%84%9c-glsl-%ec%bd%94%eb%93%9c%eb%a5%bc-%ec%89%bd%ea%b2%8c-%ed%8f%ac%ed%95%a8%ed%95%b4-%eb%b3%b4%ec%9e%90/'
dsq_thread_id:
  - "3861237280"
categories:
  - C++
  - OpenGL
tags:
  - glsl
---
C/C++ 에서 GLSL 코드를 삽입하려면 &#8220;&#8221; 와 \n의 남발로 코드가 아래처럼 지져분해 진다.

<pre class="lang:c decode:true">static const char * vertexShader =
"#version 300 es                                          \n"
"in vec4        VertexPosition;                           \n"
"in vec4        VertexColor;                              \n"
"uniform float  RadianAngle;                              \n"
"out vec4       TriangleColor;                            \n"
"mat2 rotation = mat2(cos(RadianAngle),sin(RadianAngle),  \
                     -sin(RadianAngle),cos(RadianAngle)); \n"
"void main() {                                            \n"
"  gl_Position   = mat4(rotation)*VertexPosition;         \n"
"  TriangleColor = VertexColor;                           \n"
"}\n";</pre>

매크로를 정의해서 아래처럼 쉽고 예쁘게 넣어보자. 물론 버전은 사용하는 GLSL 버전에 알맞게 수정한다.<!--more-->

<pre class="lang:default decode:true">#define GLSL(str) (char*)"#version 330 es\n" #str</pre>

<pre class="">static const char * vertexShader = GLSL(
    in vec4        VertexPosition;
    in vec4        VertexColor;
    uniform float  RadianAngle;
    out vec4       TriangleColor;

    mat2 rotation = mat2(cos(RadianAngle),sin(RadianAngle),
                        -sin(RadianAngle),cos(RadianAngle));

    void main() {
        gl_Position   = mat4(rotation)*VertexPosition;
        TriangleColor = VertexColor;
    }
);
</pre>

<pre class="lang:default decode:true ">static const char * fragmentShader = GLSL(
    precision mediump float;
    in vec4 TriangleColor;
    out vec4 FragColor;
    void main() {
      FragColor = TriangleColor;
    }
);</pre>

코드편집기가 문법 하일라이팅을 지원하면 GLSL 코드에도 적용되어 읽기가 편하고 수정하기도 쉽다.

한 가지 문제점이 있는데 바로 GLSL코드 내에서 매크로를 사용해야 할 경우이다. 안드로이드 카메라를 개발할 경우 카메라 프리뷰에 GLSL코드를 적용하기 위해서 GL\_OES\_EGL\_image\_external 확장을 사용할 필요가 있다. 또한 같은 GLSL코드로 오프스크린 렌더링으로 GLSL코드가 적용된 이미지를 얻고자 사용할 때는 GL\_TEXTURE\_2D를 사용해야 한다. 이럴 경우에는 전처리문을 사용해야 하는데 위의 GLSL 매크로를 그대로 이용하면 원하는 결과가 나오지 않는다. 이럴 경우 어떻게 해결해야 할까?

<pre class="lang:default decode:true ">#define GLSL(code) ((char*)( #code ))</pre>

<pre class="lang:default decode:true">GLSL(
    varying highp vec2 textureCoordinate;

    \n#ifdef USE_EXTERNAL_OES_TEXTURE\n
    \n#extension GL_OES_EGL_image_external : require\n
    uniform samplerExternalOES inputImageTexture;
    \n#else\n
    uniform sampler2D inputImageTexture;
    \n#endif\n

    void main()
    {
        gl_FragColor = texture2D(inputImageTexture, textureCoordinate);
    }
);</pre>

위처럼 전처리문을 \n &#8230; \n 으로 감싸주면 된다.

> 출처 : <http://youku.io/questions/287709/gcc-stringification-and-inline-glsl>