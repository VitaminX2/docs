# Neural Light Field 

> *"Vision is about the question of building representations."*  
> *"We (humans) likely have a 3D inductive bias."*  
> *"All computer vision should be 3D computer vision, because our world is a 3D world.*  
> *- Vincent Sitzmann -*

## 들어가며,

Deep Learning기반 Light Field 과제를 제안합니다.

Real World는 3D 입니다. 시간 축까지 생각하면 4D 겠네요.

기존 카메라의 Imaging은, 이 Continuous Space-Time으로 이루어진 4D Real World를 특정 시간, 특정 시점에서 Discrete Sampling한 2D Projection에 불과합니다. 궁극의 Imaging은 결국 4D Continuous Space-Time을 Capture 해 내는 것이라고 생각합니다.


Light Field는 이 관점에서 흥미로운 분야입니다. 우리는 공간에 존재하는 빛(Ray)을 눈으로 Capture하여 사물을 인식(Recognition)합니다. 만약 우리가 공간에 존재하는 모든 Ray에 대한 정보를 가지고 있다면, 원하는 시간에, 원하는 장소에서 그 공간을 복원(Reconstruction) 할 수 있습니다. 기존 2D 카메라는 이러한 기능이 없습니다. 반면에 Light Field는 공간상의 한 점을 통과하는 Ray를 빛의 진행 방향, 시간 등의 변수로 이어진 고차원 함수로 표현합니다.

![lightfield](./idea-challenge/lightfield.png)

즉, 기존 2D 카메라가, $I(x,y)$ 와 같은 함수로 표현된다면, Light Field는 $L(x,y,z,\theta,\phi,t)$ 로 생각할 수 있습니다.

Light Field는 어떤 기능을 제공할 수 있을까요? Light Field는 실제로 2D Imaging과 *문자 그대로 차원이 다른 개념*이라서, ;-)
차원이 다른 기대 효과들이 있습니다.
* Captured 영상의 **Focus를 변경**할 수 있습니다.
* Captured 영상의 **Depth of Field를 조절**해서 마치 대형 렌즈로 촬영한 듯 배경을 흐리게 만들수도, Smartphone으로 촬영한듯 배경을 Sharp하게 만들 수 있습니다.
* Captured 영상의 **Perspective도 조절**할 수 있습니다.
* 완벽하게 **다른 시점에서 촬영한 영상으로 변경**할 수 있습니다.
* 아예 **3D Imaging**을 할 수도 있습니다.

이미 다수의 야구 중계에서 위 기능 중 일부가 활용되고 있어서, 스포츠 팬분들은 아마 어떤 기능일지 감이 오실 겁니다.

![baseball](./idea-challenge/baseball4d.png)

결국 더 많은 차원(Dimension)의 정보를 Capture하여 더 높은 Controllability를 가지게 되고, 이를 통해 전에 없던 새로운 사용자 경험을 제공하는 것이 핵심입니다.

## Google Starline Project

지난 5월 18일 Google은 [Project Starline](https://blog.google/technology/research/project-starline)을 공개했습니다.
![Starline](./idea-challenge/google_starline.png)

이 프로젝트는 멀리 떨어져있는 사람들이 마치 서로 실제 눈앞에 보고 있는 것처럼 실물 Scale의 Light Field Display를 제공하는 것을 목표로 하고 있습니다. 핵심 기술은 3D Capturing과 Real-time Compression을 이용한 실시간 전송, 그리고 3D Imaging을 통한 실감나는 상호 Communication의 실현입니다.
한마디로 표현하면, **Telepresence**입니다. 처음 이 용어가 세상에 소개 된 것은 1942년의 Science Fiction에서라고 하니, 그동안 기술의 발전이 얼마나 기대에 못미쳤는지 따져볼만 하네요. ;-)

 
## 본론 

결국, 이 문서는 Light Field Capturing & Imaging을 하자는 내용이 될 것입니다.


Light Field는 기존 카메라로는 Capture 할 수 없기 때문에, ~~글을 읽다가 이미 눈치채신 분들이 많겠지만~~ 위의 야구중계 사진처럼 수많은 카메라를 Time-synchronize해서 촬영하여 생성하게 됩니다.

이런 식으로는 일반 사용자가 Light Field를 Caputure하고 Light Field Display (혹은 VR기기 ;-))에서 이를 감상할 수 있는 날은 오지 않겠죠. 1942년에서 이미 약 80년이나 흘렀는데...

**제안하는 과제는 Light Field Caputure를 가능하게 하는 특수한 Camera를 만들고자 하지 않습니다.
반대로 모든 사람들이 가지고 있는 Smartphone을 Light Field Camera가 되도록하는 AI S/W Project를 진행하는 것을 제안합니다.**

물론, 다수의 카메라를 써서라도 Light Field 그 자체를 정밀하게 Capture하는 것은 매우 중요한 연구이고 이 연구도 매우 뜨겁습니다.
관심이 있으시다면, 아래 프로젝트 페이지를 소개드립니다.
[Immersive Light Field Video with a Layered Mesh Presentation by Google @ SIGGRAPH 2020](https://augmentedperception.github.io/deepviewvideo/)

하지만, 자사 Business 관점에서는 Practical한 S/W Solution에 관심을 가져볼만 합니다.

현재 Smartphone은 Pixel Resolution은 이미 1억에 육박하며, Multi-camera를 기본으로 가지고 있습니다.
만약 사용자가 이런 Smartphone을 이용해 Video Sequence를 촬영하면, 우리는 Time Synchonization은 없지만 Multi-View Image를 얻을 수 있습니다. 이러한 정보를 조합하여 Neural Network 학습을 통해 Light Field Function을 Reconstruction하도록 S/W System을 학습하면,
Smartphone을 Portable Light Field Capture 장비로 활용 할 수 있지 않을까요?

## 실현가능성

이 과제와 가장 관련성이 높은 연구는 최근 Computer Vision과 Machine Learning 분야에서 굉장히 뜨거운 관심을 받고 있는,
Neural Radiance Field (NeRF)입니다. 이 연구는 3D 공간 상의 Ray의 Radiance값을 Direction 별로 다르게 Encoding하여 $F(x,y,z,\phi,\theta)$ 형태로 표현합니다. Light Field와 굉장히 유사하죠?

![NeRF](./idea-challenge/nerf.png)

NeRF는 여러 View의 2D Image를 모아서, 하나의 통일된 Radiance Field를 Reconstruction합니다. 이를 통해 미처 보지 못했던 새로운 시점(Novel View)의 영상도 생성해 낼 수 있게됩니다. Novel View 생성은 5D Space에 대한 일종의 Volume Rendering으로 이루어집니다.

하지만 이 분야는 2020년에 비로소 시작되어서, 넘어야할 많은 문제들을 안고 있습니다.
몇가지만 뽑아보면 아래와 같습니다.

* Test-time Optimization: 기본적으로 매 Scene 마다 Optimization을 수행해야 하는 한계
* Efficient Computing: 5D 입력에 대한 Ray 값을 얻기 위해, 매번 Neural network를 Inference해야 하는 한계
* Mobile Device에 적용 가능성: 학계는 아무도 고려하고 있지만, 우리는 반드시 해결해야 하는 문제

제안하는 과제의 실질적 Deliverable은 Light Field Capturing을 수행하는 Smartphone S/W Solution이며,
연구 과제로서는, 위의 문제들에 대해 답을 내는 과정이 되겠습니다.

## 마치며,

여기까지 읽어보시면 별 알맹이는 없는데 라고 생각하고 계시겠죠? 저도 그렇게 생각합니다. ;)
그래도 짬내서 끝까지 읽어주셔서 진심으로 감사드립니다. 
읽어보시는 분들께서 조금이나마 편안했으면 하는 마음에, 문투는 약간 누그러서 작성했습니다. 양해부탁드립니다. 

감사합니다.  
강나협 드림.
