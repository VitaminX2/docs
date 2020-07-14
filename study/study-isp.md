# Note on ISP study of sensor division

## 센서의 정석: ISP

### The Nature of Light

* 파장에 대해서 어떤 에너지 분포 표현 - SPD
* Daylight condition D65광은 이렇고, 형광등은 이렇고

### Blackbody

* 색온도는 흑체복사랑 관련이 있음.
* 표면온도에따라 Emission color가 바뀜.

### Color Temperature

* 

### Color in Human Eye

* 4가지 셀
* Cone셀, Rod셀
* 세가지 색 느끼는 콘이 시신경 중심으로 많고
* 밝기를 느끼는 로드가 주변에 많다.
* 사람눈도 가시광선에 반응하는데 에너지가 낮을수록 빨간색, 높을수록 파란색으로 색을 인지

### subjective terms for color

* 색공간
* RGB, YUV, Lab, XYZ
* HSV -- Munsell Color System요건 사람이 이해 좋은데...
  * Chroma (saturation)
* ISP는 YUV를 많이 쓰는데... 
  * color와 밝기가 분리된 장점
  * 사람눈에 거슬리지 않음.

### Basic ISP: overview

* Bayer Processing (Demosacing 전까지)
* Color Processing (Demosaicing 이후부터)

### Basic ISP: Color Filter Array

* minic the physiology of the human eyes

### Basic ISP: Open Raw Data

* 17년 기준 mobile 10bit, automotive 12bit

### Basic ISP: Removing Pedestal

* 기본적으로 Gain 값을 Green에는 먹이지 않습니다.

### Bilinear Interpolation

* 3x3으로 심플하게 했는데,
* ISP는 기본적으로 5x5해서 Edge방향등을 고려해서 처리함.


### Sharpening Quality Check - MTF


## 센서의 정석: FE-ISP 기초의 이해






## 참고 - AP Schedule

### I. Color & ISP Pipeline 기초 – Seminar Tutorial	담당자
Week 1	Color theory/Color Space
- ICCV tutorial (M. Brown, Part I)	박성헌

Week 2	ISP Pipeline & Processing
- ICCV Tutorial (M. Brown Part II)	최재석

Week 3	ISP more
- 센서 사업팀 교육 자료 (센서의 정석, CIS Seminar-Chapter6. ISP)
- 센서 사업팀 교육 자료 (센서의 정석, FE-ISP 기초_이해)	강나협

Week 4	E2E ISP & Mobile2DSLR, Intro
- Raw2DSLR Replacing Mobile Camera ISP with a Single Deep Learning Model, arXiv, 2020
- DeepISP: Towards Learning an End-to-End Image Processing Pipeline, IEEE TIP, 2019	서건석

Week 5	Metric w/ Percetual Metric
- The Unreasonable Effectiveness of Deep Features as a Perceptual Metric, CVPR, 2018
- The Perception-Distorsion Tradeoff (CVPR 2018)
- NIMA: Neural Image Assessment, IEEE TIP 2018
- Learned Perceptual Image Enhancement, arXiv 2017	이수진

Week 6	Toward AI ISP from Human
- 센서 사업팀 교육 자료 (센서의 정석, Human Perception and Deep Learning Based Quality Assessment)
- Eye to Brain Signal Processing (Perception & Control Mechanism)	강은희

### II. In-Depth Image Processing – Recent Paper	담당자

Week 7	Color Processing - AWB - Learning-based methods (w/ Deep Learning) : 
- Convolutional Color Constancy, ICCV 15)
- Fast Fourier Color Constancy (CVPR 17)
- FC4 (CVPR 17)
- Quasi-Unsupervised Color Constancy (CVPR 19)
- Cascading Convolutional Color Constancy (AAAI 2020)

[Optional]
Color Processing – AWB (Parametric or Statistics-based Approach)
- White-patch, Gray-world
- Edge-Based Color Constancy (TIP 07)
- Cheng et al. (JOSA 14)
- Efficient Illuminant Estimation for Color Constancy Using Grey Pixels (CVPR 15)
- Revisiting Gray Pixel for Statistical Illumination Estimation (VISAPP 19)	(곽영준)

Week 8	Color Processing – (Multi-) Illumination Estimation
- Multiple illumination
: Color Constancy Algorithm for Mixed-illuminant Scene Images (Access 17)
: Color Constancy for Multiple Light Sources (TIP 12)
: Combining Bottom-Up and Top-Down Visual Mechanisms for Color Constancy Under Varying Illumination (TIP 2019)
: Conditional GANs for Multi-Illuminant Color Constancy (CVPRW 19)
- Illumination Estimation
: Deep Reflectance Maps (CVPR 16)
: Deep Outdoor Illumination Estimation (CVPR 17)
: All-Weather Deep Outdoor Lighting Estimation (CVPR 19)
: Underexposed Photo Enhancement using Deep Illumination Estimation (CVPR 19)
: Deep Parametric Indoor Lighting Estimation (ICCV 19)	이한아
최지호

Week 9	Mobile to DSLR 
- DSLR-Quality Photos on Mobile Devices with Deep Convolutional Networks, ICCV, 2017
- PIRM Challenge, ECCVW, 2018
- NTIRE Challenge, CVPRW, 2019
- Deep Photo Enhancer: Unpaired Learning for Image Enhancement from Photographs with GANs, CVPR, 2018
- WESPE: Weakly Supervised Photo Enhancer for Digital Cameras, CVPRW 2018	이솔애

Week 10	Raw Data Processing
- Zoom to Learn, Learn to Zoom, CVPR, 2019
- Towards Real Scene Super-Resolution with Raw Images, CVPR, 2019
- Unprocessing Images for Learned Raw Denoising, CVPR, 2019
- A High-Quality Denoising Dataset for Smartphone Cameras, CVPR 2018
- Deep Joint Demosaicking and Denoising, ACM ToG, 2016	권기남

Week 11	Perceptual Metric More 이수진

Week 12	센서 사업팀 교육 자료 추가
- Team Wiki – Image Sensor의 이해 & Image Sensor Noise
- Deep learning을 활용한 vision sensor

[Optional] AWB + Color space transform
- Beyond White (ICCV 15)
- Improving Color Reproduction Accuracy on Cameras (CVPR 18)
- When Color Constancy Goes Wrong (CVPR 19)
- Deep White-Balance Editing (CVPR 20 Oral)	(이상원)
