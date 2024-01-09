---
title: "디지털 이미지 처리 - Color Space"
date: 2022-01-16 23:30:00 +0900
categories: [dev]
tags: [imaging]
---
# 디지털 이미지 처리

## 목차
- [디지털 이미지 처리](#디지털-이미지-처리)
  - [목차](#목차)
  - [서론](#서론)
  - [기본 용어 설명](#기본-용어-설명)
    - [A. Color Model (색상 모델)](#a-color-model-색상-모델)
    - [B. Color Space (색공간)](#b-color-space-색공간)
  - [1. RGB 색상 모델](#1-rgb-색상-모델)
  - [2. CMYK 색상 모델](#2-cmyk-색상-모델)
  - [3. HSL / HSV 색상 모델](#3-hsl--hsv-색상-모델)
    - [3-1. HSL(HSB): 색조(Hue) + 채도(Saturation) + 밝기(Lightness)](#3-1-hslhsb-색조hue--채도saturation--밝기lightness)
    - [3-2. HSV: 색조(Hue) + 채도(Saturation) + 명도(Value; Brightness)](#3-2-hsv-색조hue--채도saturation--명도value-brightness)
  - [4. YUV 색상 모델](#4-yuv-색상-모델)
  - [5. CIERGB / CIEXYZ / CIELAB 색상 모델](#5-ciergb--ciexyz--cielab-색상-모델)
  - [References](#references)

## 서론

현재 컴퓨터 그래픽스를 다루는 회사를 다니면서 다양한 색상공간 간 변환에 대해 한번 정리를 해야겠다고 생각을 했었다.

## 기본 용어 설명

### A. Color Model (색상 모델)

- Color Model(색상 모델)은 색상을 수학적으로 나타내기 위한 틀이다.
- 즉, 하나의 색을 몇 가지의 성분을 가지고, 어떻게 표현할 것인지를 나타내는 추상적인 모델이다.
- RGB 색상 모델은 디스플레이의 R, G, B에 해당하는 빛 신호의 양을 조절하여 색을 표현하는 방법을 의미한다.

### B. Color Space (색공간)

- Color Space(색공간)은 Color Model(색상 모델)에서 정의한 색상을 모아서 만든 집합을 의미한다.
- 8비트 depth의 RGB 색상 공간은 R,G,B가 0에서 255사이  
(8비트, 2^8=256이므로 구성 성분의 특징을 256가지로 표현할 수 있다)의 정수 값을 갖는 순서쌍의 집합  
{(R, G, B)|0<=R,G,B<=255 where R, G, and B are integers}  
로 나타낼 수 있다.

## 1. RGB 색상 모델
> 가산혼합: 색을 합하면 밝아지는 색혼합 방식(빛의 삼원색을 혼합하여 색을 구성하는 디스플레이에서 주로 사용되는 방식)

![RGB 색상 모델](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/AdditiveColor.svg/440px-AdditiveColor.svg.png)

- <span style="color: red">빨간색(Red)</span> + <span style="color: green">초록색(Green)</span> + <span style="color: blue">파란색(Blue)</span>

- 특징: 빨간색 빛, 초록색 빛, 파란색 빛을 합성하여 색상 중추를 자극하여 만드는 색 모델이다. (물론, 인간이 인지할 수 있는 모든 색상 영역을 만들어내지는 못한다.)  
빛은 섞을수록 그 세기가 더해지기 때문에 RGB 색상 모델은 가산 색상 합성 모델이다.  

- 문제점: RGB의 R, G, B가 실제 정확히 어떤 색인지에 대해서는 합의된 부분이 없기 때문에 같은 R, G, B 값을 가진 색이라도 디스플레이마다 다른 색으로 보일 수 있다.

- RGB 색상 공간 종류(정해놓은 R, G, B의 기준 값이 다르다)
    - sRGB
    - Adobe RGB
    - ProPhoto RGB
    - scRGB
    - CIE RGB

## 2. CMYK 색상 모델
> 감산혼합: 색을 합하면 어두워지는 색혼합 방식(컬러 프린터와 같은 출력 매체에서 색을 구성하는 방식)

![CMYK 색상 모델](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c9/CMYK_subtractive_color_mixing.svg/300px-CMYK_subtractive_color_mixing.svg.png)

- <span style="color: cyan">옥색(Cyan)</span> + <span style="color: magenta">자홍색(Magenta)</span> + <span style="color: yellow">노랑색(Yellow)</span> + <span style="color: black">검정색(Key, black)</span>

- 특징
    - 4가지 색의 잉크(옥색, 자홍색, 노랑색, 검정색)로 전체 또는 부분을 칠해 백지로 된 배경을 가리는 방식으로 색을 표현하는 모델이다.  
    - 잉크를 칠하게 되면, 빛이 반사되는 양이 줄어들기 때문에 잉크를 섞는 것은 곧 어두워짐(감산혼합)을 의미한다.  
    - 혹자는 검정색을 만들기 위해 K라는 성분 없이 C, M, Y의 색을 합해서 만들면 되지 않겠냐라는 반문을 할 수 있다.  
    - 그런데 검정색에 해당하는 Key라는 성분이 따로 있는건 잘 이해되지 않을 수 있다.
    - **여기서 경제적인 이유가 등장한다.**  
        1. 검정색 잉크를 따로 넣어서 사용하는 것은 서로 다른 색의 잉크를 섞어 검정색을 만드는 것보다 사용하는 잉크 비용을 절약할 수 있기 때문이다.  
        (왜냐하면, 검정색 잉크의 가격이 더 저렴하기 때문이다.)  
        2. 더 풍성한 검정색을 표현하거나 채도가 없는 어두운 색을 표현하는데도 검정색 잉크만을 사용하는 것이 더 용이하다.
        3. 잉크를 많이 써서 검정색을 만들면 잉크가 마르는데도 시간이 오래 걸린다.  
        4. 사실 인간의 시각 체계를 잘 살펴보면 색상 간 차이를 구분하는 원추 세포의 갯수보다 밝기 차이를 구분하는 간상 세포의 개수가 더 많기 때문에 글자를 읽거나 구분하는 용도로 검정색의 역할은 매우 중요하다.

## 3. HSL / HSV 색상 모델

HSL과 HSV은 이름이나 하는 일이 비슷하여 같은 것이라고 착각하기 쉽지만 엄연히 **<u>다른</u>** 색상 모델이다.

### 3-1. HSL(HSB): 색조(Hue) + 채도(Saturation) + 밝기(Lightness)

![HSL 색상 모델](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b3/HSL_color_solid_dblcone_chroma_gray.png/394px-HSL_color_solid_dblcone_chroma_gray.png)

> HSL은 도색공들이 서로 다른 페인트를 섞어 색을 만드는 과정을 모델링한 색상 모델이다.

- 특징: 가령, 밝은 빨간색을 만들때 흰색 페인트(밝기: Lightness)에 빨간 색소(색상: Hue)를 넣어 만들 수 있는 것처럼 색상을 구성할 수 있다.

### 3-2. HSV: 색조(Hue) + 채도(Saturation) + 명도(Value; Brightness)

![HSV 색상 모델](https://upload.wikimedia.org/wikipedia/commons/thumb/0/00/HSV_color_solid_cone_chroma_gray.png/394px-HSV_color_solid_cone_chroma_gray.png)

> HSV는 물체에 조명을 비출 때 물체가 띄고 있는 색이 어떻게 보이는가를 모델링한 색상 모델이다.

- 특징
    - HSL과의 차이점은 HSL은 L(밝기)가 최대일 때 순수한 흰색을 나타내지만, HSV는 어떤 색을 띄는 물체에 빛을 비출때 보이는 물체의 색이므로 순수한 흰색만 나타내지는 않는다.

- 문제점
    - 하지만 HSL나 HSV 모두 인간의 시각 인지 능력을 고려하지 않은 모델이므로 때문에 밝기(L 또는 V)를 고정한 상태로 채도(S)를 변경하게 되면 밝기가 일정하지 않은 것처럼 보인다.

## 4. YUV 색상 모델

![YUV 색상 모델](https://upload.wikimedia.org/wikipedia/commons/thumb/2/29/Barn-yuv.png/300px-Barn-yuv.png)
> 위에서부터 차례대로 원본 이미지, Y값, U값, V값 성분을 표시한 것이다.
- 특징
    -
- 문제점
    -

- YUV 색상 공간
    - YIQ
    - YUV
    - YDbDr
    - YPbPr
    - YCbCr

## 5. CIERGB / CIEXYZ / CIELAB 색상 모델
> CIE(Commission internationale de l'éclairage) 국제 조명 위원회를 프랑스어로 써서 줄인 말이다.  

- CIE 색상공간
    - CIERGB

    CIE 색공간은 빛 신호의 세기(RGB)나 그 외의 방법으로 색을 만드는 방법(CMYK, HSL, HSV, YUV)을 기준으로 구성한 다른 색공간과 달리 인간의 색채 지각 능력을 기반으로 구성한 색공간이다.  
    인간의 색채 지각 능력을 기준으로 색공간을 어떻게 구성할 수 있었을까?  

    **사실 방법은 놀랍게도 단순하다.**  

    색감각이 매우 좋은 실험자 ~~(좋다의 기준이 매우 모호하지만...)~~ 에게 임의의 색(시험색)을 보여주고 직접 원색광 3개(R, G, B)의 밝기를 조절하여 해당 색과 동일한 색을 만들어내도록 시켰다.  

    물론 이 방법으로도 눈에 보이는 모든 색을 만들어낼 수 없었다.  

    이 경우에는 3개의 원색 중 하나를 시험색에 더한 뒤 나머지 두 색을 합성하여 동일한 색을 만들어내도록 하여 시험색에 더해진 색은 음의 값을 갖는 것으로 처리할 수 있다.  

    그럼 각각의 색상에 대한 삼원색의 밝기를 그래프(색 대응 함수)로 나타내어 인간의 색채 지각 능력을 반영한 색공간을 구성할 수 있다.  

    ![색 대응 함수](https://upload.wikimedia.org/wikipedia/commons/thumb/6/69/CIE1931_RGBCMF.svg/650px-CIE1931_RGBCMF.svg.png)
    > CIE 1931 RGB 색 대응 함수.  
    > 색 대응 함수는 가로축의 파장에 해당하는 단색광과 같은 색을 대응시키기 위해 필요한 삼원색의 양을 나타낸다.

    - CIEXYZ

    CIERGB 공간상의 점들을 0~1 사이 값들로 매핑하여 색공간을 구성한 것

    - CIELAB

    CIEXYZ 는 색공간 위의 인접 색상간 차이가 균일하지 않아 균일하게 만든 색공간

    - CIELUV

---

## References

1. [https://en.wikipedia.org/wiki/Color_space](https://en.wikipedia.org/wiki/Color_space)
2. [https://en.wikipedia.org/wiki/Brightness](https://en.wikipedia.org/wiki/Brightness)
3. [https://en.wikipedia.org/wiki/Color_model](https://en.wikipedia.org/wiki/Color_model)
4. [https://en.wikipedia.org/wiki/RGB_color_model](https://en.wikipedia.org/wiki/RGB_color_model)
5. [https://en.wikipedia.org/wiki/CMYK_color_model](https://en.wikipedia.org/wiki/CMYK_color_model)
6. [https://en.wikipedia.org/wiki/Key_plate](https://en.wikipedia.org/wiki/Key_plate)
7. [https://en.wikipedia.org/wiki/HSL_and_HSV](https://en.wikipedia.org/wiki/HSL_and_HSV)
8. [https://en.wikipedia.org/wiki/YUV](https://en.wikipedia.org/wiki/YUV)
9. [https://en.wikipedia.org/wiki/CIELAB_color_space](https://en.wikipedia.org/wiki/CIELAB_color_space)