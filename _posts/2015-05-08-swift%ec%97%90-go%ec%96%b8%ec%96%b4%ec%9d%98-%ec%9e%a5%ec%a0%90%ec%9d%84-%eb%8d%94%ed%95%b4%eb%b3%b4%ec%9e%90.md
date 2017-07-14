---
id: 418
title: Swift에 Go언어의 장점을 더해보자
date: 2015-05-08T20:05:42+00:00
author: Burt
layout: post
guid: http://blog.burt.pe.kr/?p=418
permalink: '/swift%ec%97%90-go%ec%96%b8%ec%96%b4%ec%9d%98-%ec%9e%a5%ec%a0%90%ec%9d%84-%eb%8d%94%ed%95%b4%eb%b3%b4%ec%9e%90/'
dsq_thread_id:
  - "3746925396"
categories:
  - Go
  - Swift
tags:
  - channel
  - go
  - goroutine
  - swift
---
따끈한 오픈소스 소식. [goswift](https://github.com/tidwall/goswift)!

Go언어의 장점을 Swift언어에 더해 iOS/Mac 프로젝트에서 Go언어의 장점을 사용할 수 있다. 추가한 Go언어의 기능은 아래와 같다.<!--more-->

  * Goroutines
  * Defer
  * Panic, Recover
  * Channels 
      * Buffered Channels
      * Select, Case, Default
      * Closing
  * Sync Package 
      * Mutex, Cond, Once, WaitGroup
  * Error type

swift로 고루틴과 채널을 사용하는 예제는 아래와 같다.

<pre class="lang:swift decode:true">func main() {
    var jobs = Chan&lt;Int&gt;(buffer: 5)
    var done = Chan&lt;Bool&gt;()

    go {
        for ;; {
            var (j, more) = &lt;? jobs
            if more {
                println("received job \(j!)")
            } else {
                println("received all jobs")
                done &lt;- true
                return
            }
        }
    }

    for var j = 1; j &lt;= 3; j++ {
        jobs &lt;- j
        println("sent job \(j)")
    }
    close(jobs)
    println("sent all jobs")

    &lt;-done
}</pre>

아래 주소에 좀 더 많은 예제가 있으니 살펴보자.

  * <https://github.com/tidwall/goswift/tree/master/examples>