---
id: 651
title: Swift와 C++ 코드 동시에 사용하기
date: 2016-02-23T06:04:03+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=651
permalink: '/swift%ec%99%80-c-%ec%bd%94%eb%93%9c-%eb%8f%99%ec%8b%9c%ec%97%90-%ec%82%ac%ec%9a%a9%ed%95%98%ea%b8%b0/'
dsq_thread_id:
  - "4602485607"
categories:
  - C++
  - iOS
  - ObjectiveC
  - Swift
tags:
  - cpp
  - objectivec
  - swift
---
ObjectiveC는 C언어의 Super Set이라 C코드를 자유롭게 ObjC 파일 내에서 사용할 수 있습니다. C++의 경우 m 확장자를 mm으로 변경해 주면 사용할 수 있습니다. iOS의 개발 언어가 Swift로 변경되고 나서는 C++을 사용하기 위해 약간의 수고를 더 해줘야 합니다. 안드로이드의 JNI에 비하면 아무 것도 아니지만요.

다음 순서로 하면 됩니다.

  1. C++ 모듈을 만든다 (h/cpp or hpp/cpp)
  2. ObjC++ 랩퍼를 만든다. (h/mm)
  3. 브릿지헤더를 만든다.
  4. 사용한다.

아주 간단한 예제를 작성해 볼게요.

## C++ 모듈을 만든다 (h/cpp or hpp/cpp) {#toc_1}

아래와 같은 C++ 클래스를 생성합니다. 그냥 단순하게 ItsCpp라 지었어요. name 속성을 입력 받고 반환하는 메서드가 있습니다.

<pre class="lang:c++ decode:true ">#ifndef ItsCpp_hpp
#define ItsCpp_hpp

#include &lt;string&gt;

class ItsCpp {
    
public:
    ItsCpp();
    ItsCpp(const std::string& name);
    ~ItsCpp();
    
public:
    // set/get name property
    void name(const std::string& name);
    const std::string& name();
    
private:
    std::string m_name;
};



#endif /* ItsCpp_hpp */</pre>

이제 클래스를 구현합니다.

<pre class="lang:c++ decode:true ">#include "ItsCpp.hpp"

ItsCpp::ItsCpp() {
    name("");
}

ItsCpp::ItsCpp(const std::string& name) {
    this-&gt;name(name);
}

ItsCpp::~ItsCpp() {
}

void ItsCpp::name(const std::string &name) {
    this-&gt;m_name = name;
}

const std::string& ItsCpp::name() {
    return this-&gt;m_name;
}</pre>

## ObjC++ 랩퍼를 만든다. (h/mm) {#toc_2}

ObjC++랩퍼에스는 데이터의 변환 등을 주의해서 해야 합니다. 특히 문자열은 ObjC가 유니코드 문자인 반면에 C++는 그렇지 않으니까요 UTF로 인코딩해줘야 합니다.

<pre class="lang:objc decode:true ">#import &lt;Foundation/Foundation.h&gt;

@interface ObjCppWrapper : NSObject
- (instancetype)initWithName:(NSString *)name;
- (NSString *)getName;
- (void)setName:(NSString *)name;
@end</pre>

구현해 줍니다.

<pre class="lang:objc decode:true ">#import "ObjCppWrapper.h"
#include "ItsCpp.hpp"

@interface ObjCppWrapper()
@property ItsCpp *itsCpp;
@end

@implementation ObjCppWrapper
- (instancetype)initWithName:(NSString *)name {
    if (self = [super init]) {
        self.itsCpp = new ItsCpp(std::string([name cStringUsingEncoding:NSUTF8StringEncoding]));
    }
    return self;
}

- (NSString *)getName {
    return [NSString stringWithUTF8String:self.itsCpp-&gt;name().c_str()];
}

- (void)setName:(NSString *)name {
    self.itsCpp-&gt;name(std::string([name cStringUsingEncoding:NSUTF8StringEncoding]));
}

- (void)dealloc {
    delete _itsCpp;
    _itsCpp = nil;
}
@end</pre>

## 브릿지헤더를 만든다. {#toc_3}

<pre class="lang:c++ decode:true ">//SwiftAndCpp-Bridging-Header.h
#import "ObjCppWrapper.h"</pre>

## 사용한다. {#toc_4}

<pre class="lang:swift decode:true ">import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let itsCppWrapper = ObjCppWrapper(name: "김성철")
        print(itsCppWrapper.getName())
        itsCppWrapper.setName("Burt.K")
        print(itsCppWrapper.getName())
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}</pre>

## 디버깅이 편리해요! {#toc_5}

안드로이드에서 NDK 레이어를 디버깅하려면 정말 힘들어요. 이것저것 해줘야 할 것도 많고 ObjC언어가 C언어의 슈퍼셋이라는 이점때문에 XCode에서 아주 쉽게 C,C++코드를 디버깅할 수 있습니다.

![]({{ site.url }}/wp-content/uploads/2016/02/debug_cpp.png){:.fit-width}
## 예제 프로젝트

  * <https://github.com/skyfe79/UsingSwiftAndCpp>

## 관련글

  * [Swift와 C언어의 Pointer](http://blog.burt.pe.kr/swift%ec%99%80-c%ec%96%b8%ec%96%b4%ec%9d%98-pointer/)