---
id: 673
title: Swift로 즐기는 이미지프로세싱
date: 2016-03-16T01:25:12+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=673
permalink: '/swift%eb%a1%9c-%ec%a6%90%ea%b8%b0%eb%8a%94-%ec%9d%b4%eb%af%b8%ec%a7%80%ed%94%84%eb%a1%9c%ec%84%b8%ec%8b%b1/'
dsq_thread_id:
  - "4665069459"
categories:
  - Swift
tags:
  - image processing
  - swift
---
Swift와 Playground를 즐길만한 놀이를 찾다가 간략하게 픽셀을 가지고 노는 걸 해보면 어떨까해서 만들어 보았습니다. 거창하게 이미지프로세싱이라 제목을 붙였지만 아주 간단한 것들만 해 보았습니다. 🙂 우연히 발견한 RGBAImage가 많은 도움이 되었습니다. 아래의 Playground는 <https://github.com/skyfe79/SwiftImageProcessing> 에서 받을 수 있습니다.

### Swift Image Processing

This project contains swift playgrounds that demonstrate how to do pixel operations in swift.

### Thanks to RGBAImage

  * <http://mhorga.org/2015/10/19/image-processing-in-ios-part-3.html>
  * This articles help me a lot!

### Convert UIImage to RGBA Image

RGBAImage has pixels flat memory. You can access pixels with index directly.

![](http://i2.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/01_rgba_image.png?w=665&#038;ssl=1){:.fit-width}

### Contrast

This is example for pixel operation

![](http://i0.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/02_contrast.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">let rgba = RGBAImage(image: UIImage(named: "monet")!)!

var totalR = 0
var totalG = 0
var totalB = 0

rgba.process { (pixel) -&gt; Pixel in
    totalR += Int(pixel.R)
    totalG += Int(pixel.G)
    totalB += Int(pixel.B)
    return pixel
}

let pixelCount = rgba.width * rgba.height
let avgR = totalR / pixelCount
let avgG = totalG / pixelCount
let avgB = totalB / pixelCount

func contrast(image: RGBAImage) -&gt; RGBAImage {
    image.process { (var pixel) -&gt; Pixel in
        let deltaR = Int(pixel.R) - avgR
        let deltaG = Int(pixel.G) - avgG
        let deltaB = Int(pixel.B) - avgB
        pixel.R = UInt8(max(min(255, avgR + 3 * deltaR), 0)) //clamp
        pixel.G = UInt8(max(min(255, avgG + 3 * deltaG), 0))
        pixel.B = UInt8(max(min(255, avgB + 3 * deltaB), 0))

        return pixel
    }
    return image
}
let newImage = contrast(rgba).toUIImage()</pre>
  
  <p>
    &nbsp;
  </p>
</div>

### Grab color space

#### Grab Red component

![](http://i0.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/03_grab_r.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">func grabR(image: RGBAImage) -&gt; RGBAImage {
    var outImage = image
    outImage.process { (var pixel) -&gt; Pixel in
        pixel.R = pixel.R
        pixel.G = 0
        pixel.B = 0
        return pixel
    }
    return outImage
}</pre>
  
  <p>
    &nbsp;
  </p>
</div>

#### Grab Green component

![](http://i0.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/04_grab_g.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true">func grabG(image: RGBAImage) -&gt; RGBAImage {
    var outImage = image
    outImage.process { (var pixel) -&gt; Pixel in
        pixel.R = 0
        pixel.G = pixel.G
        pixel.B = 0
        return pixel
    }
    return outImage
}</pre>
  
  <p>
    &nbsp;
  </p>
</div>

#### Grab Blue component

![](http://i0.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/05_grab_b.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true">func grabB(image: RGBAImage) -&gt; RGBAImage {
    var outImage = image
    outImage.process { (var pixel) -&gt; Pixel in
        pixel.R = 0
        pixel.G = 0
        pixel.B = pixel.B
        return pixel
    }
    return outImage
}</pre>
</div>

#### Compose RGB Color components

![](http://i1.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/06_composite.png?w=665&#038;ssl=1){:.fit-width}

<pre class="lang:default decode:true ">public static func composite(rgbaImageList: RGBAImage...) -&gt; RGBAImage {
    let result : RGBAImage = RGBAImage(width:rgbaImageList[0].width, height: rgbaImageList[0].height)
    for y in 0..&lt;result.height {
        for x in 0..&lt;result.width {

            let index = y * result.width + x
            var pixel = result.pixels[index]

            for rgba in rgbaImageList {
                let rgbaPixel = rgba.pixels[index]
                pixel.Rf = min(pixel.Rf + rgbaPixel.Rf, 1.0)
                pixel.Gf = min(pixel.Gf + rgbaPixel.Gf, 1.0)
                pixel.Bf = min(pixel.Bf + rgbaPixel.Bf, 1.0)
            }

            result.pixels[index] = pixel
        }
    }
    return result
}</pre>

### RGB to Gray

![](http://i2.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/07_rgb_to_gray.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">func gray5(image: RGBAImage) -&gt; RGBAImage {
    var outImage = image
    outImage.process { (var pixel) -&gt; Pixel in
        let result = sqrt(pow(pixel.Rf, 2) + pow(pixel.Rf, 2) + pow(pixel.Rf, 2))/sqrt(3.0)
        pixel.Rf = result
        pixel.Gf = result
        pixel.Bf = result
        return pixel
    }
    return outImage
}
let rgba5 = RGBAImage(image: UIImage(named: "monet")!)!
gray5(rgba5).toUIImage()</pre>
  
  <p>
    &nbsp;
  </p>
</div>

### Refactoring Split Color Space

![](http://i0.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/08_split.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">public static func splitRGB(rgba: RGBAImage) -&gt; (ByteImage, ByteImage, ByteImage) {
    let R = ByteImage(width: rgba.width, height: rgba.height)
    let G = ByteImage(width: rgba.width, height: rgba.height)
    let B = ByteImage(width: rgba.width, height: rgba.height)

    rgba.enumerate { (index, pixel) -&gt; Void in

        R.pixels[index] = pixel.R.toBytePixel()
        G.pixels[index] = pixel.G.toBytePixel()
        B.pixels[index] = pixel.B.toBytePixel()
    }

    return (R, G, B)
}</pre>
  
</div>

`ByteImage` has only one color component.

### Images ADD, SUB, MUL, DIV

![](http://i1.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/09_add_sub_mul_div.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">public static func op(functor : (Double, Double) -&gt; Double, rgbaImage1: RGBAImage, rgbaImage2: RGBAImage) -&gt; RGBAImage {
    let result : RGBAImage = RGBAImage(width:rgbaImage1.width, height: rgbaImage1.height)
    for y in 0..&lt;result.height {
        for x in 0..&lt;result.width {

            let index = y * result.width + x
            var pixel = result.pixels[index]

            let rgba1Pixel = rgbaImage1.pixels[index]
            let rgba2Pixel = rgbaImage2.pixels[index]


            pixel.Rf = min(functor(rgba1Pixel.Rf, rgba2Pixel.Rf), 1.0)
            pixel.Gf = min(functor(rgba1Pixel.Gf, rgba2Pixel.Gf), 1.0)
            pixel.Bf = min(functor(rgba1Pixel.Bf, rgba2Pixel.Bf), 1.0)

            result.pixels[index] = pixel
        }
    }
    return result

}
public static func add(rgba1: RGBAImage, _ rgba2: RGBAImage) -&gt; RGBAImage {
    return op((+), rgbaImage1: rgba1, rgbaImage2: rgba2)
}</pre>
  
  <p>
    &nbsp;
  </p>
</div>

![](http://i0.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/10_add_sub_mul_div.png?w=665&#038;ssl=1){:.fit-width}

### Blending

![](http://i1.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/11_blending.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">public static func blending(img1: RGBAImage, _ img2: RGBAImage, alpha: Double) -&gt; RGBAImage {
    let result : RGBAImage = RGBAImage(width:img1.width, height: img1.height)
    for y in 0..&lt;result.height {
        for x in 0..&lt;result.width {

            let index = y * result.width + x
            var pixel = result.pixels[index]

            let pixel1 = img1.pixels[index]
            let pixel2 = img2.pixels[index]


            pixel.Rf = min( alpha * pixel1.Rf + (1.0 - alpha) * pixel2.Rf, 1.0)
            pixel.Gf = min( alpha * pixel1.Gf + (1.0 - alpha) * pixel2.Gf, 1.0)
            pixel.Bf = min( alpha * pixel1.Bf + (1.0 - alpha) * pixel2.Bf, 1.0)

            result.pixels[index] = pixel
        }
    }
    return result
}</pre>
  
  <p>
    &nbsp;
  </p>
</div>

### Brightness

![](http://i2.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/12_brightness.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">public static func brightness(img1: RGBAImage, contrast: Double, brightness: Double) -&gt; RGBAImage {
    let result : RGBAImage = RGBAImage(width:img1.width, height: img1.height)
    for y in 0..&lt;result.height {
        for x in 0..&lt;result.width {

            let index = y * result.width + x
            var pixel = result.pixels[index]

            let pixel1 = img1.pixels[index]

            pixel.Rf = min( pixel1.Rf * contrast + brightness, 1.0)
            pixel.Gf = min( pixel1.Gf * contrast + brightness, 1.0)
            pixel.Bf = min( pixel1.Bf * contrast + brightness, 1.0)

            result.pixels[index] = pixel
        }
    }
    return result
}</pre>
  
  <p>
    &nbsp;
  </p>
</div>

### Convolution

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">public static func convolution(var image: ByteImage, mask: Array2D&lt;Double&gt;) -&gt; ByteImage {

    let height = image.height
    let width  = image.width

    let maskHeight = mask.rowCount()
    let maskWidth  = mask.colCount()

    for y in 0..&lt;height - maskHeight + (maskHeight-1)/2 {
        for x in 0..&lt;width - maskWidth + (maskWidth-1)/2 {
            var v = 0.0
            if (y+maskHeight &gt; height) || (x+maskWidth) &gt; width {
                continue
            }

            for my in 0..&lt;maskHeight {
                for mx in 0..&lt;maskWidth {
                    let tmp = mask[my, mx]
                    v = v + (image.pixel(x+mx, y+my)!.Cf * tmp)
                }
            }

            v = clamp(v, lower: 0.0, upper: 1.0)
            print(v)
            let pixel = BytePixel(value: v)
            let xx = x+(maskWidth-1)/2
            let yy = y+(maskHeight-1)/2
            image.setPixel(xx, yy, pixel)
        }
    }
    return image
}</pre>
  
  <p>
    &nbsp;
  </p>
</div>

#### Sharpening

![](http://i1.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/13_sharpening.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">let m1 = Array2D(cols:3, rows:3,
    [ 0.0/5.0, -1.0/5.0, 0.0/5.0,
     -1.0/5.0,  9.0/5.0,-1.0/5.0,
      0.0/5.0, -1.0/5.0, 0.0/5.0])
ImageProcess.convolution(R.clone(), mask: m1).toUIImage()</pre>
  
  <p>
    &nbsp;
  </p>
</div>

#### Bluring

![](http://i2.wp.com/github.com/skyfe79/SwiftImageProcessing/raw/master/art/14_blur.png?w=665&#038;ssl=1){:.fit-width}

<div class="highlight highlight-source-swift">
  <pre class="lang:default decode:true ">let m2 = Array2D(cols: 3, rows: 3,
    [
         1.0/9.0, 1.0/9.0, 1.0/9.0,
         1.0/9.0, 1.0/9.0, 1.0/9.0,
         1.0/9.0, 1.0/9.0, 1.0/9.0,
    ]
)
ImageProcess.convolution(R.clone(), mask: m2).toUIImage()</pre>
  
  <p>
    &nbsp;
  </p>
</div>