# Script

:boy: 안녕하세요. Photography의 미래에 대해 말씀드리려고 나온 CVL 강나협,  
:girl: MPL 손민정입니다. :arrow_right:  

## Intro.

### 어원

사진이란 무엇일까요 :arrow_right:  
Photography의 어원을 살펴보면, :arrow_right:  
빛을 그리다 라는 의미가 담겨있습니다. :arrow_right:  
고대로부터 사진은 말그대로 빛을 Capture하고 그대로 모사하는 Rendering 과정으로 이루어집니다. :arrow_right:  

### 역사

1827년 최초의 사진이 등장하고 :arrow_right:  
1878년 최초의 비디오가 나왔습니다. :arrow_right:  

이후 사진은 :arrow_right:    
흑백에서 Color로 ...  
필름에서 Digital로 ...  
Smartphone으로 눈부시게 발전해왔습니다.

특히, 사람들이 사진을 소비하는 방식 역시, 통신망의 발전에 따라 달라져왔는데,  
5G, 6G를 이야기하는 지금, 다음 세대의 사진은 어떤 형태여야 할까요? :arrow_right:  


### Our World is 3D and Dynamic
(문구/비디오를 A.K꺼로 교체해서 넣어본다.)

저희는 그것이 4D Space-Time Photography 라고 생각합니다.
왜냐하면 우리가 살고 있는 세상이 매우 Dynamic한 3차원이기 때문이죠. :arrow_right:  

### 4D Space-Time Photography "Intel Video"

4D Space-Time Photography는 수많은 카메라를 이용해서 모든 방향의 장면을 촬영하고 조합하여 생성됩니다.  
그 결과로, **시간과 공간에 대한 완전한 Control**이 가능해집니다.  :movie_camera: :arrow_right:  

하지만 이게 전부가 아닙니다. :arrow_right:  

### 4D Space-Time Photography "MS Video"

시공간에 대한 완벽한 이해는 그 다음 단계를 가능하게 하는데요.
대표적인 것이 Lighting 즉, 빛에 대한 Control입니다.
Photography의 개념인 빛을 Capture하고 Rendering한다는 관점에서는 
가장 본질에 가까운 형태라고 생각되지 않으십니까? :arrow_right:  


### Evidence

:arrow_right:  
많은 매체와 :arrow_right:  
업체들도 :arrow_right:  :arrow_right:  :arrow_right:  
이 예측에 동의하고 준비하고 있습니다. :arrow_right:  


### Photo: Capture the light

빛을 Capture하고, Rendering하는 원리에 대해 "잠깐만" 살펴보겠습니다. :arrow_right:  
먼저 소개드릴 내용은, Photo 즉 Capture the light에 대한 내용입니다. :arrow_right:  

### Plenoptic Function

Plen Optic Function은 
Complete physics of light & Vision이란 의미로 :arrow_right:  
모든 순간, :arrow_right:  
모든 방향, :arrow_right:  
모든 시점에 대한 4D Space-Time의 빛의 정보를 저장하는 개념입니다.
그야말로, 빛을 Capture한다는 관점의 최종 형태라고 볼수있겠습니다. :arrow_right:  

#### Hard to capture?
하지만 이 모든 시공간을 캡춰한다는 것은 사실 불가능합니다. :arrow_right:  

### Need Camera Array
그나마, 현실적인 접근은 많은 수의 카메라 Array를 사용하는 것입니다. :arrow_right:  


### Huge Memory Footprint

이렇게 만들어져도 Function의 크기가 어마어마하다는 문제가 있습니다.
128x128 해상도를 가정해도 약 4.4TB의 저장 공간이 필요하죠. :arrow_right:  

### Graphy: Render the light

다음은 :arrow_right:   
Graphy 즉, Render the light에 대한 이론입니다. :arrow_right:  

### Rendering Equation

빛을 **그린다**는 관점에서 중요한 원리는 Rendering Equation 입니다.  
조명을 출발한 빛이 눈에 도착하기까지의 여정을 모두 담아낸 수식인데요. :arrow_right:  

빛은 눈에 바로 도착할수도 있고, :arrow_right:  
또는 중간에 다른 물체를 만나 변형되어 도착할 수도 있습니다.  
이를 완벽하게 모델링하기 위해서는 :arrow_right:  
물체의 반사특성, :arrow_right:  
조명 정보, :arrow_right:  
그리고 물체의 형상 정보가 필요합니다.  
우리가 만약 이들을 모두 알고 있다면, 어떤 장면의 빛도 계산할 수 있습니다. :arrow_right:  

#### Hard to obtain

하지만, 우리는 이러한 요소들이 상호작용된 결과만 관찰 할 수 있습니다.  
따라서, 이러한 Physical Attribute를 획득하는 것은 매우 어려운 문제입니다. :arrow_right:  


### Need Light Stage

이를 위해서는, Light Stage처럼 수십개의 조명과 카메라가 동기화된 특수한 장비 등이 필요합니다. :arrow_right:  

### Challenge

그런데 사람들이 Camera Array나 Light Stage를 들고 다닐 순 없겠죠.  
결국 4D Space-Time Photography의 실현을 위해서는  
효율적이고 통합된 고차원 표현을  
Smartphone만으로 수행할 수 있는 새로운 방법론이 필요합니다. :arrow_right:  

## [간지] Our Approach

이를 위해, 저희는 :arrow_right:   

### 전략 Theory
이 두가지 방법론을 융합한 :arrow_right:   
새로운 방향을 제시하고자 합니다.

제안하는 Framework는 Neural Representation과 Neural Rendering으로 구성됩니다. :arrow_right:  
Neural Representation은 PlenOptic Function을 Physically Correct하면서, 효율적 저장이 가능하게 합니다. :arrow_right:  
그리고, Neural Rendering은 Data-driven으로 Physical Attribute를 추정하고,  
이를 통한 물리 기반의 영상 생성을 실현합니다. :arrow_right:  

~~정리하면, PlenOptic Function은 실제 세상을 “수많은 카메라로 측정하여” 빛을 Capture하는 데이터 기반 방법론이고,
Rendering Equation은 “Light Stage등을 통해 획득한” 빛의 여정을 연산하여 그려내는 물리기반 방법론 입니다.~~




### 4D Space-Time과 Neural Representation & Rendering

이제, 4D Space-Time을 Neural Representation과 Neural Rendering으로 어떻게 표현할 수 있을지 말씀드리겠습니다.

먼저, Neural Representation은, 
PlenOptic Function과 마찬가지로 :arrow_right:  
위치, 방향, 시간 입력을 받아, :arrow_right:  
색상과 밀도를 출력으로 하는 :arrow_right:  
Neural Network으로 정의하겠습니다.  
그러면 우리는 연속적인 6차원 정보를 단지 5MB의 저장공간으로 “충분히” 표현할 수 있습니다. :arrow_right:  

그리고 Neural Rendering은 Neural Representation을 Query하여 :arrow_right:  
빛의 경로에 따라, 임의의 시점에서 바라본 Pixel 정보를 계산합니다. :arrow_right:  

결국 두 모듈의 상호작용을 통해 4D Space-Time을 효율적으로 저장하고,  
원하는 시점, 순간의 영상 생성이 가능해집니다. :arrow_right:  

### Decomposition & Synthesis 

Space-Time Control이 문제의 끝이 아니라고 말씀드렸었는데요.

어떤 장면의 빛에 대한 정보를 모두 알게 된다면, 우리는 이를 :arrow_right:  
조명 정보, :arrow_right:  
반사 정보, :arrow_right:  
형상 정보로 분해하여,
Rendering Equation에 기반하여 더 많은 자유도를 제공할 수 있을 것입니다.

이를 실현하기 위해, 분해된 요소들을 재조합하여 Neural Representation을 복원하도록 학습하는 전략을 제안합니다.  
그 결과, 임의의 View 뿐만 아니라 Lighting까지 자유롭게 Control하여 영상 생성이 가능해질 것입니다. :arrow_right:  

### Smartphone Issues

이를 Smartphone에서 실현하려면, 몇가지 문제가 더 있습니다.  

사용자가 Smartphone으로 자유롭게 촬영하려면 저희는 카메라의 좌표계 정보없이 이 문제를 풀어야 합니다.  
이를 해결하기 위해 Neural Representation과 Camera Pose를 Joint Optimization으로 풀고자 합니다.  

정말 큰 문제는 Computation 한계입니다.   
Mobile GPU는 아무래도 성능 한계가 있기 때문에,  
Network 경량화, Basis Decomposition, Precomuted Caching 등 다양한 접근을 시도할 필요가 있습니다. :arrow_right:  

### Future of Smartphone Photography

Smartphone Photography의 Pipeline 측면에서는 어떨까요? :arrow_right:  

### Smartphone Photography: From capture to display

현재의 Smartphone은 ISP가 Sensor 신호를 처리해 Image로 저장하고,  
Display 할 때에는 GPU에서 다시 Pixel로 복원하여 그려주는 매우 단순한 구조입니다.  
4D Space-Time Photography를 실현하기 어려운 Framework이죠. :arrow_right:  

다행스럽게도, 최근 여러 개의 초고해상도 후면 카메라, Dual-Pixel Sensor가 적용되어,  
PlenOptic Function의 핵심요소인 다중 시점 Capture에는 큰 도움이 될 것으로 보입니다. :arrow_right:  

여기에 Representation과 :arrow_right:   
Rendering을 모두 Neural Processing으로 수행하기 위해서는, :arrow_right:  
기존의 ISP와 GPU의 역할이 :arrow_right:  
작아지고 NPU의 처리 능력을 극한까지 활용하는 새로운 Framework가 되어야 합니다.  

최적의 Solution을 위해,  
현재 Smartphone의 NPU 역할을 확장하여 Capture부터 Display까지 NPU가 중심이 되는  
새로운 SoC 설계가 병행되어 시너지를 내어야 할 것입니다. :arrow_right:  

## Outro

4D Space-Time Photography가 가져올 미래를 상상해 보시죠.  

2D 평면을 넘어서,  
Photography가 본질적으로 추구하는 바 그대로  
“Dynamic한 3D 세상”을 Immersive하게 체험하는 새로운 미래가 펼쳐질 것입니다.  
그리고, 이는 그간의 Photography의 역사에서 봐왔듯이 새로운 Business를 개척하고 확장할 것입니다.  

:movie_camera:
:arrow_right:  

저희는 Neural Representation & Rendering이 그 새로운 세상의 문을 열어주리라 확신합니다. :arrow_right:  

## Ending Credit

함께 준비해준 박성헌, 문재영 전문입니다.  
감사합니다.



