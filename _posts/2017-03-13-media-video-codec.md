---
layout: post
title: "media basic video codec"
date:   2017-03-13 03:25:50 +0900
categories: info
tags: info media video codec 
---


# Video 란? 

짧은 시간 간격 동안 순차적으로 표시되는 정지화면들의 집합

# Video 특성

- 이웃한 frame 간의 유사성이 존재한다.  
- 많은 양의 Data가 필요.  

  example) 640*480 해상도, RGB 24-bit 칼라의 data bandwidth 
    
  * 1/30sec :640 * 480 * 24 bits = 7.03 Mbits
  * 1sec    : 	640 * 480 * 24 bits * 30 frames = 210.9 Mbps
  * 90min   :210.9 Mbps * 90 분 * 60 초 = 1112.2 Gbits 

# 압축의 필요성. 

  - cf ) 저속의 인터넷 대역폭 : 10Mbps or 100Mbps
  - cf ) 24 배속 CD-ROM의 입출력 대역폭:153.6KB * 24 배속 * 8 bits = 28.8 Mbps

# 압축 방법 

* intra coding : frame 자체에 대한 coding 
* inter coding : 인접한 frame간의 유사성을 갖는 비디오의 특성을 이용. 

# INTRA CODING

Jpeg이 대표적인 intra coding의 예이다. 아래와 같은 기법을 사용하여 손실을 포함한 압축을 시도한다.

![](https://www.researchgate.net/profile/Massimo_Vecchio/publication/220199850/figure/fig1/AS:276960686166027@1443043981458/Fig-1-Modules-of-the-JPEG-algorithm.png)


* Color Space의 변경 (YUV)

  RGB는 각 성분에 밝기를 포함 하기 때문에 중복이 발생하게 된다. 이 중복을 제거한 YUV.
  YCbCr은 digital 영역에서 사용되는 정의이지만 YUV로 혼용되어 사용되기도 한다. 
  흑백 TV 협회(NTSC, PAL)에서 기존 TV망을 그대로 사용하기를 원하기 때문이기도 하다. 

   * Y : 밝기 정보 
   * U : R - Y (r에서 밝기 제거)
   * V : B - Y (b에서 밝기 제거)

* DCT

  주파수 공간을 활용한 압축. Spatial doamin의 data를 frequency domain으로 변환해 보면 data 주위의 값은 대부분 비슷하여 변화가 작다. 즉, low frequency가 많다라고 해석 할 수 있다. 

* Quantization 

  DCT의 결과값을 특정 값들로 나누어 축소화. 이부분에서 loss 발생. 압축률의 핵심이 되는 부분. 미리 정의된 Quantization Table(codec 의 성능, level)을 사용하여 

* Zig-Zag Scanning 

  위의 영역에서 0의 중복이 많이 보인다. 이를 줄이기 위해 Matrix를 zig-zag로 표현 하여 좌상단에 모여있는 숫자쓰고 0인 부분을 생략하면, data를 상당 부분 줄일 수 있다. 

* Entropy Coding

  Run-length coding, Huffman coding

# INTER CODING 

각각의 frame은 상당한 유사성을 가지고 있으로, 이를 이용하여 차이값만 가지고 encoding을 진행

* Motion Estimation & Motion Compensation

  ![](https://users.cs.cf.ac.uk/Dave.Marshall/Multimedia/Topic5.fig_150.gif)

# CODEC

* Level : codec의 encoding, decoding 성능에 따른 분류
* Profile :  codec에서 지원하는 기능의 사용유무에 따른 구분

  ex) H.264

  ![](http://cfile25.uf.tistory.com/image/27425C4A575F1ABC2818A7)


# TERMS

* Interlace / Progressive

  * Interlace scan : 과거 CRT의 한 frame을 화면에 보여 줄 수 있는 bandwidth의 물리적인 한계가 있었다. 이를 극복하기 위해 한 frame을 2개의 frame으로 분리하여 화면에 보여주는 방식을 말한다. 

  * Progressive scan : 일반적인 방식. 

# ref

* https://www.researchgate.net/


my second commits.
