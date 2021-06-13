# Narrative of TED

## SNS

SNS의 Trend를 살펴보면, Tweeter가 140자 단문 형태의 Communication을 시작한 이래로, Facebook, Instagram, YouTube 그리고 최근 엄청나게 성장한 Tic-Toc까지
시장과 사용자들은 Text, Image, Video의 형태로 나름대로의 짧은 길이라는 형태를 유지하면서 Media의 차원을 확장시켜가고 있습니다.
Facebook이 집중하고 있는 VR과 Metaverse도 SNS의 차원을 확장하는 연장선으로 볼수 있을거 같습니다.

* Tic-Toc의 시가총액


## 미세픽셀

삼성은 Image Pixel의 초미세화를 통한 Mobile Formfactor에서 인간 눈에 필적하는 초 고해상도 영상 해상도를 목표로 연구와 개발을 진행하고 있습니다.
아마도, 2025년 쯤에는 6억화소급 모바일 Image Sensor가 탄생할 거라고 저도 기대하고 있습니다.
그런데, 사용자들은 이러한 초고화소 영상 센서를 요구하고 있을까요?

YouTube와 Instagram 혹 Tic-Toc에서 제공하는 해상도를 한편 살펴봅니다.

* 증거 제시: 미세픽셀 센서 발표시기와 SNS 등 미디어의 해상도 지원 수준에 대한 비교/정리 차트?

물론 Device의 한계때문에 저기에 머물러 있는 것일 수도 있습니다.

## Kodak과 Digital Camera

잠시 다른 사례를 살펴볼까요?
최초로 사용화된 Digital Camera의 해상도를 기억하시나요? 그 당시의 Digital Camera의 해상도는 필름 카메라에 필적했을까요?

* 증거 제시: 최초의 Digital Camera 해상도, 필름카메라의 환산 해상도

## Future of Photography

초 고해상도 이미지 센서 물론 중요하고 핵심적인 기술입니다. 하지만, 혹시 우리가 놓치고 있는 것은 있지 않을까요?
이미지 센서, 카메라가 궁극적으로 하고자 하는 일은 세상을 Capture 하는 일입니다.

* Plenoptic Function에 대한 개념 설명
* There is no goat, only light

Plenoptic Function은 실현되면 그야말로 모든 순간, 모든 시점의 이미지를 생성할 수 있게되므로 궁극의 Principle이라는 느낌을 줍니다.

* Plenoptic이 되면...

그러나, 현실적으로 이러한 Capture를 해내는 것은 사실 불가능합니다.
하지만, Computational Photography의 많은 연구자들과 몇몇 Pioneer들이 참신한 시도를 했고, 실패했거나 실패하는 중입니다. 

* 실패한 Commercial Light Field Cameras
  * Lytro, Pelican Imaging, L16, 
* 여전히 계속되는 연구와 시도들 
  * 구글, 애플, Facebook, Sony?
  * MLA와 Multicamera 구조 그림
  * Intel Studio?

기존 카메라로는 불가능하다는 새로운 기능을 제시하고 있지만, 실패한 이유는 무엇일까요?
* 실패 분석: 카메라의 본질적 기능에 대한 완성도, 사용 편의성, 새로운 카메라를 추가로 들고 다녀야 함.

## Smartphone & Plenoptic

오늘 저희가 제시하고자 하는 방향은 조금 다릅니다. 결론부터 말씀드리면, 새로운 카메라를 만드는 것이 아닙니다.
현재 스마트폰을 살펴볼까요?
이미지 센서 1억화소, 후면의 Multicamera... 스마트폰은 아마 의도한건 아니었겠지만, 아마도 카메라의 본질적 기능을 더 잘 제공하기 위한 연장선이었겠죠?
그렇지만, 그렇게 해서 도착한 현재의 스마트폰의 형태는 이미 Portal Light Field 카메라가 가져야 할 특징들을 가지고 있습니다.

사용자가 Casual하게 Smartphone을 이용해서 동시에 Multicamera를 이용한 Video를 Capture하고 나면, 마치 거대한 Light Field Camera System으로 촬영한 것과 같은 기능을 제공하는 것
그것을 가능하게 하는 Software Solution 과제를 제시하고자 합니다.

## Neural Representation

이 새로운 카메라 S/W Solution이 가지는 가장 큰 특징은 Multiple Video로 Capture된 초 고해상도 영상들을 조합하여 촬영 Scene에 대한 종합적인 Representation을 Neural Network을 이용해 표현하는 것입니다.
* Neural Network as a universal approximator for Plenoptic Function
* ACORN, SIREN, 그리고 NeRF 소개
* 넘어야할 Research Challenges

## S/W Architectural View

*센서부터 Display까지 하나의 Flow로 연결할때, 제안하는 S/W Solution은 기존 대비 무엇을 어떻게 바꾸는지 제시함.
이게 바로 우리의 Deliverable이고, Vision이다.
Road Map도 자연스럽게 녹여내면 좋다.*

## 맺음말

여러분 GPU로 Graphics 처리를 할수 있다는 사실 아시나요? (농담)

* NVIDIA의 GPU와 CUDA 사례소개

Computation Platform 제시를 통한 선구적 기업. S/W와 H/W Platform의 이상적 Synergy 그러한 Device Business의 Impact
카메라는 GPU/Display의 반대편에서 비슷한 Process가 역순으로 일어나는 곳.
이 과제가 제사하는 영상 데이터 포맷의 Neural Representation으로의 변화는 단순히 S/W Solution이 아니라 이를 실현하기 위한 Sensor/Processor와 Synergy이고, 새로운 변화를 불러 일으킬 가능성이 충분하다.

어쩌면, 2030년에는 사람들에게 Camera ISP가 2D Image만 처리했다는 사실 아시나요? 라는 농담을 하게 될지도 모르겠습니다.
