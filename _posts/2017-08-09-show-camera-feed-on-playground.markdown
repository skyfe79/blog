---
layout: post
title:  "플레이그라운드에서 카메라피드 보여주기"
permalink: /show-camera-feed-on-playground/
date:   2017-08-09 00:00:00 +0900
categories: projects
---

Xcode 플레이그라운에서 실시간으로 카메라 피드를 보여줄 수 있을까? iOS용 플레이그라운에서는 불가능하지만 MacOS용 플레이그라운드에서는 가능하다. iOS용 플레이그라운드는 플레이그라운드를 실행할 때 시뮬레이터를 사용한다. 그러나 iOS용 시뮬레이터는 현재까지 실시간 카메라피드를 지원하지 않는다. MacOS환경에서 카메라피드를 실시간으로 플레이그라운드에 보여주는 예제를 작성해 보자.

### MacOS 플레이그라운드를 만든다.

![]({{ site.url }}/assets/images/show-camera-feed-on-playground/img01.png){: .fit-width }

### 캡쳐세션구성하기

실시간 카메라피드를 만들기 위해서 AVFoundation 캡쳐 세션을 구성해야 한다. 캡쳐세션을 구성하는 단계는 아래와 같다.

![]({{ site.url }}/assets/images/show-camera-feed-on-playground/captureOverview_2x.png){: .fit-width }
[이미지출처](https://developer.apple.com/library/content/documentation/AudioVideo/Conceptual/AVFoundationPG/Articles/04_MediaCapture.html#//apple_ref/doc/uid/TP40010188-CH5-SW2)

1. 세션을 생성한다.
2. 캡쳐입력을 생성한다.
3. 캡쳐출력을 생성한다.
4. 캡쳐출력 프리뷰를 설정한다.(선택사항)

이 단계를 따라 코드를 작성해 보자.

우선 필요한 프레임워크를 임포트한다.

```swift
import Cocoa
import AVFoundation
import PlaygroundSupport
```

#### 캡쳐세션 만들기

```swift
let session = AVCaptureSession()
session.beginConfiguration()
session.sessionPreset = AVCaptureSessionPreset640x480
session.commitConfiguration()
```

캡쳐세션 구성을 설정할 때는 반드시 `beginConfiguration`과 `commitConfiguration` 블럭 내에서 설정해야 한다.

#### 캡쳐입력 만들기

```swift
var input : AVCaptureDeviceInput

// 원하는 디바이스 찾기
if let devices = AVCaptureDevice.devices() as? [AVCaptureDevice] {
    for device in devices {
        if device.hasMediaType(AVMediaTypeVideo) && device.supportsAVCaptureSessionPreset(AVCaptureSessionPreset640x480) {
            do {
                input = try AVCaptureDeviceInput(device: device)
                // 찾은 디바이스를 세션에 추가한다.
                if session.canAddInput(input) {
                    session.addInput(input)
                    break
                }
            } catch {
                print(error)
            }
        }
    }
}
```

캡쳐입력을 만들 때는 원하는 기능을 갖춘 디바이스를 찾아서 캡쳐입력을 만들어야 한다. 만든 캡쳐입력을 만든 다음에는 세션에 추가한다.

#### 캡쳐출력 만들기

```swift
let output = AVCaptureVideoDataOutput()
output.videoSettings = [kCVPixelBufferPixelFormatTypeKey as AnyHashable : kCVPixelFormatType_32BGRA]
output.alwaysDiscardsLateVideoFrames = true

if session.canAddOutput(output) {
    session.addOutput(output)
}
```

출력할 비디오 프레임의 픽셀 포맷을 설정한다. 지연된 프레임은 버리도록 설정한다.

#### 프리뷰 레이어 만들기

```swift
let captureLayer = AVCaptureVideoPreviewLayer(session: session)

let view = NSView(frame: NSRect(x: 0, y: 0, width: 640, height: 480))
view.wantsLayer = true
view.layer = captureLayer
```

프리뷰 레이어를 만든 후 NSView 레이어로 설정했다.

#### 플레이그라운드 liveView 설정하기

```swift
PlaygroundPage.current.liveView = view
```

#### 세션 시작하기

```swift
session.startRunning()
```

### 결과

![]({{ site.url }}/assets/images/show-camera-feed-on-playground/img02.png){: .fit-width }

### 전체 코드

```swift
//: Playground - noun: a place where people can play

import Cocoa
import AVFoundation
import PlaygroundSupport

//1. 세션 만들기
//-------------------------------------------------------------
let session = AVCaptureSession()
session.beginConfiguration()
session.sessionPreset = AVCaptureSessionPreset640x480
session.commitConfiguration()

//2. input 만들기
//-------------------------------------------------------------
var input : AVCaptureDeviceInput

//3. 원하는 디바이스 찾기
if let devices = AVCaptureDevice.devices() as? [AVCaptureDevice] {
    for device in devices {
        if device.hasMediaType(AVMediaTypeVideo) && device.supportsAVCaptureSessionPreset(AVCaptureSessionPreset640x480) {
            do {
                input = try AVCaptureDeviceInput(device: device)
                if session.canAddInput(input) {
                    session.addInput(input)
                    break
                }
            } catch {
                print(error)
            }
        }
    }
}

//4. output 만들기
//-------------------------------------------------------------
let output = AVCaptureVideoDataOutput()
output.videoSettings = [kCVPixelBufferPixelFormatTypeKey as AnyHashable : kCVPixelFormatType_32BGRA]
output.alwaysDiscardsLateVideoFrames = true

if session.canAddOutput(output) {
    session.addOutput(output)
}

//5. 캡쳐레이어 구성하기
//-------------------------------------------------------------
let captureLayer = AVCaptureVideoPreviewLayer(session: session)

let view = NSView(frame: NSRect(x: 0, y: 0, width: 640, height: 480))
view.wantsLayer = true
view.layer = captureLayer
PlaygroundPage.current.liveView = view

//6. 세션시작하기
//-------------------------------------------------------------
session.startRunning()
```