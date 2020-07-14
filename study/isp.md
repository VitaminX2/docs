# Issues on ISP

## Wikipedia

**Image restoration** is the operation of taking a corrupt/noisy image and estimating the clean, original image.
**Image restoration** is performed by reversing the process that blurred the image and such is performed by imaging a point source and use the point source image, which is called the **Point Spread Function (PSF)** to restore the image information lost to the blurring process.

**Image restoration** is different from **image enhancement** in that the latter is designed to emphasize features of the image that make the image more pleasing to the observer, *but not necessarily to produce realistic data* from a scientific point of view.

The objective of image restoration techniques is to **reduce noise** and **recover resolution loss**.
The most straightforward and a conventional technique for image restoration is deconvolution, which is performed in the frequency domain and after computing the Fourier transform of both the image and the PSF and undo the resolution loss caused by the blurring factors.
This deconvolution technique, because of its direct inversion of the PSF which typically has poor matrix condition number, amplifies noise and creates an imperfect deblurred image.


## Research Items

### DB

* Color Image Denoising on Kodak24
* Color Image Denoising on BSD68
* Color Image Denoising on Darmstadt Noise Dataset
* Low-Light Image Enhancement on AFLW (Zhang CVPR 2018 crops)
* Low-Light Image Enhancement on MEF
* Low-Light Image Enhancement on 3DMatch Benchmark
* Low-Light Image Enhancement on LOL
* Deblurring on REDS
* Video Super-Resolution on Vid4
* Image Enhancement on MIT-Adobe 5k


### Restoration

* A Bayesian Perspective on the Deep Image Prior, CVPR 2019
* Deep Image Prior, CVPR 2018
* Noise2Noise: Learning Image Restoration without Clean Data, ICML 2018
* **DeblurGAN-v2: Deblurring (Orders-of-Magnitude) Faster and Better, ICCV 2019**
* **EnlightenGAN: Deep Light Enhancement without Paired Supervision, ???**
* Enhancing Intrinsic Adversarial Robustness via Feature Pyramid Decoder, CVPR 2020
* CycleISP: Real Image Restoration via Improved Data Synthesis, CVPR 2020
* Replacing Mobile Camera ISP with a Single Deep Learning Model, https://arxiv.org/abs/2002.05509v1
* HighEr-Resolution Network for Image Demosaicing and Enhancing, ICCV 2019 W/S, AIM 2019 Raw2RGB Challenge Winner
* Modulating Image Restoration with Continual Levels via Adaptive Feature Modification Layers, CVPR 2019
* CFSNet: Toward a Controllable Feature Space for Image Restoration, ICCV 2019
* Deep Network Interpolation for Continuous Imagery Effect Transition, CVPR 2019
* The Perception-Distortion Tradeoff
* Residual Non-local Attention Networks for Image Restoration, ICLR 2019


### Denoising

* Unprocessing Images for Learned Raw Denoising, CVPR 2019
* Supervised Raw Video Denoising with a Benchmark Dataset on Dynamic Scenes, CVPR 2020
* A Physics-based Noise Formation Model for Extreme Low-light Raw Denoising, CVPR 2020
* High-Quality Self-supervised Deep Image Denoising, NeurIPS 2019
* Variational Denoising Network: Toward Blind Noise Modeling and Removal, NeurIPS 2019

### Deblurring

* Neural Blind Deconvolution Using Deep Priors, CVPR 2020
* Learning to See in the Dark, CVPR 2018
* DeblurGAN: Blind Motion Deblurring Using Conditional Adversarial Networks, CVPR 2018
* **DeblurGAN-v2: Deblurring (Orders-of-Magnitude) Faster and Better, ICCV 2019**
* EDVR: Video Restoration with Enhanced Deformable Convolutional Networks, CVPR 2019 W/S, The winners in all four tracks in the NTIRE 2019 video restoration
* Defocus Deblurring Using Dual-Pixel Data, arxiv 2020
* Comparison of Image Quality Models for Optimization of Image Processing Systems, arxiv 2020

### Demosaicking

* Replacing Mobile Camera ISP with a Single Deep Learning Model, https://arxiv.org/abs/2002.05509v1
* Trinity of Pixel Enhancement: a Joint Solution for Demosaicking, Denoising and Super-Resolution, https://arxiv.org/abs/1905.02538v1
* Deep Image Demosaicking using a Cascade of Convolutional Residual Denoising Networks, ECCV 2018
* **Reconfiguring the Imaging Pipeline for Computer Vision, ICCV 2017**
* HighEr-Resolution Network for Image Demosaicing and Enhancing, ICCV 2019 W/S, AIM 2019 Raw2RGB Challenge Winner

### HDR Reconstruction

* Single-Image HDR Reconstruction by Learning to Reverse the Camera Pipeline, CVPR 2020
* HDR image reconstruction from a single exposure using deep CNNs, SGA 2017

### Enhancement

* CURL: Neural Curve Layers for Global Image Enhancement, https://arxiv.org/abs/1911.13175v2
* Deep Photo Enhancer: Unpaired Learning for Image Enhancement from Photographs with GANs, CVPR 2018
* **EnlightenGAN: Deep Light Enhancement without Paired Supervision, ???**
* Deep Retinex Decomposition for Low-Light Enhancement, BMVC 2018 (ORAL)
* Aesthetic-Driven Image Enhancement by Adversarial Learning, https://arxiv.org/abs/1707.05251v2
* Learning an Adaptive Model for Extreme Low-light Raw Image Processing, https://arxiv.org/abs/2004.10447v1

### Image Quality Assessment

* NIMA: Neural Image Assessment, TIP 2018
* Which has better visual quality: The clear blue sky or a blurry animal?, IEEE T. Multimedia 2018
* Quality aware Generative Adversarial Networks, NeurIPS 2019

### Color Constancy

* When Color Constancy Goes Wrong: Correcting Improperly White-Balanced Image, CVPR 2019

## Schedule

### Qualcomm Snapdragon Neural Processing SDK
Status
* QC 시료폰(S20 2대) 입수 완료 (5/11)
* QC ISP Simulator 입수 완료 (5/11)
* SDS 외주 현황 설명 (5/11, 최재석전문)

Plan
Qualcomm Neural Processing SDK for AI 를 활용을 위해 검토 중
* SDK 내용 파악 및 환경 구축 (~5/15)
  * 개발자 등록, Host/Device System 환경 구축 등
* UDC Backbone Network F/U (~5/15)
* Sample Code 동작 및 분석 (~5/22)
  * SDK를 S20 시료폰에서 동작 확인 목적
* UDC Backbone Network Qualcomm Neural Processing향 변환 (~5/22)
* UDC backbone Network의 QC향 변환, 동작 및 성능 분석 (~5/29)
* 성능 한계에 따른 개선 사항 및 연구 Issue 도출 (~6/05)


## Papers on ISP

### To read

* MS Brown and SJ Kim. Understanding the in-camera image processing pipeline for computer vision. 2015 
* Seon Joo Kim, Hai Ting Lin, Zheng Lu, Sabine Susstrunk, Stephen Lin, and Michael S Brown. A new in-camera imaging model for color computer vision and its application. TPAMI, 2012
* R. Ramanath, W. E. Snyder, Y. Yoo, and M. S. Drew, “Color image processing pipeline,” IEEE Signal Processing Magazine, vol. 22, no. 1, pp. 34–43, Jan 2005.
* H. C. Karaimer and M. S. Brown, “A software platform for manipulating the camera imaging pipeline,” in European Conference on Computer Vision (ECCV), 2016.
* F. Heide, M. Steinberger, Y.-T. Tsai, M. Rouf, D. Paj ˛ak, D. Reddy, O. Gallo, J. Liu, W. Heidrich, K. Egiazarian, J. Kautz, and K. Pulli, “Flexisp: A flexible camera image processing framework,” ACM Transactions on Graphics (TOG), vol. 33, no. 6, pp. 231:1–231:13, Nov. 2014.
* G. Qian, J. Gu, J. S. Ren, C. Dong, F. Zhao, and J. Lin, “Trinity of pixel enhancement: a joint solution for demosaicking, denoising and super-resolution,” CoRR, vol. abs/1905.02538, 2019
* B. Mildenhall, J. T. Barron, J. Chen, D. Sharlet, R. Ng, and R. Carroll, “Burst denoising with kernel prediction networks,” in The IEEE Conference on Computer Vision and Pattern Recognition (CVPR), June 2018 
* G. Eilertsen, J. Kronander, G. Denes, R. K. Mantiuk, and J. Unger, “HDR image reconstruction from a single exposure using deep cnns,” ACM Transactions on Graphics (TOG), vol. 36, no. 6, pp. 178:1–178:15, 2017. 
* 

### reading
* Liu et al, "Single-Image HDR Reconstruction by Learning to Reverser the Camera Pipeline", CVPR 2020

### read
* Z. Liang, J. Cai, Z. Cao, L. Zhang, "CameraNet: A Two-Stage Framework for Effective Camera ISP Learning", arXiv 1908.01481, 2019
* S. W. Zamir, A. Arora, S. Khan, M. Hayat, F. S. Khan, M-H Yang, L. Shao, "CycleISP: Real Image Restoration via Improved Data Synthesis", CVPR 2020 (Oral)

## check
* DeepISP vs Replacing Mobile Camera ISP with a Single Deep Learning Model
