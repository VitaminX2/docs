# Note on Unsupervised Deep Visual Odometry

We will review more than three papers according to the temporal and technical orders of the publication
The papers we will cover,

* Zhou et al., Unsupervised Learning of Depth and Ego-Motion from Video, CVPR 2017
* Zhan et al., Unsupervised Learning of Monocular Depth Estimation and Visual Odometry with Deep Feature Reconstruction, CVPR 2018
* Mahjourian et al., Unsupervised Learning of Depth and Ego-Motion from Monocular Video Using 3D Geometric Constraints, CVPR 2018
* Li et al., UnDeepVO: Monocular Visual Odometry through Unsupervised Deep Learning, ICRA 2018

---

# Zhou et al., Unsupervised Learning of Depth and Ego-Motion from Video, CVPR 2017

## Abstract
* an unsupervised learning framework for the task of monocular depth & camera motion estimation from unstructured video sequences.
* requiring only monocular video sequences for training
    * single view depth net
    * multiview pose net
    * a loss based on warping nearby views to the target using the computed depth and pose
* the two nets are coupled by the loss during training
    * can be applied independently at test time

## Introduction
* Training a model that 
    * observes sequences of images to explain its observation by predicting likely camera motion and scene structure
* End-to-end
    * map directly from input pixels 
        * to an estimate of ego-motion
            * 6 DoF transformation matrix
        * and, the underlying scene structure
            * per-pixel depth maps under a reference view
* No manual labeling or even camera motion information
* Goal
    * formulate the entire view synthesis pipeline as the inference procedure of a convolutional neural network
    * meta-task of view synthesis the network is forced to learn about intermediate tasks of depth and camera pose in order to come up with a consistent explanation of the visual world.

## Approach
### View synthesis as supervision
* given one input view of a scene, syntehsize a new image of the scene seen from a different camera pose.
* a per-pixel depth + pose + visibility --> target view

$<I_1, ..., I_N>$ as a training image sequence\
$I_t$ the target view\
$I_s$ the source view\
$\hat{I}_s$ the source view warped to target coordinate frame

* taking the predicted depth $\hat{D}_t$
* the predicted $4 \times 4$ camera transformation matrix $\hat{T}_{t \rightarrow s}$

The view synthesis objective\
$\mathcal{L}_{vs}=\sum_s \sum_p |I_t(p) - \hat{I}_s(p)|$

See Figure 2

### Differentiable depth image-based rendering

$p_s \approx K\hat{T}_{t\rightarrow s}\hat{D}_t(p_t) K^{-1} p_t$

bilinear sampling is used for differentiable mapping

### Modeling the model limitaiton
* the scene is static without moving objects
* there is no occlusion/disocclusion between the target view and the source views
* the surface is Lambertian so that the photo-consistency error is meaningful.

Explainable prediction network that outputs a per-pixel soft mask $\hat{E}_s$ for each target-source pair, indicating the network's belief in where direct view synthesis will be successfully modeled for each target pixel.

$\mathcal{L}_{vs} = \sum_{<I_1, ... I_N>\in S} \sum_p \hat{E}_s(p) |I_t(p) - \hat{I}_s(p)|$

Training with above loss would result in a trivial solution of the network always predicting $\hat{E}_s$ to be zero.
Thus, we add regularization term $\mathcal{L}_{reg}(\hat{E}_s)$ that encourages nonzero predictions by minimizing the cross-entropy loss with label 1 at each pixel location

### Overcoming the gradient locality
One remaining issue with the above learning pipeline is that the gradients are mainly derived from the pixel intensity difference between $I(p_t)$ and the four neighbors of $I(p_s)$

* this would inhibit(억제하다) training if the correct $p_s$ is located in a low-texture region or far from the current estimation.

Found two strategies

* using a convolutional encoder-decoder architecture with a small bottleneck for the depth network that implicitly constrains the output to be globally smooth and facilitates gradients to propagate from meaningful regions to nearby regions
* explit multi-scale and smoothness loss that allows gradients to be delived from larger spatial regions directly.

Take the second one.

For smoothness, we minimize the $L_1$ norm of the second-order gradients for the predicted depth maps.

Final objective\
$\mathcal{L}_{final} = \sum_l \mathcal{L}^l_{vs} + \lambda_s \mathcal{L}^l_{smooth} + \lambda_e \sum_s \mathcal{L}_{reg} (\hat{E}^l_s)$


---

# Zhan et al., Unsupervised Learning of Monocular Depth Estimation and Visual Odometry with Deep Feature Reconstruction, CVPR 2018

## Abstract
* We explore the use of stereo sequences for learning depth and visual odometry
    * enables the use of both spatial(L-R) and temporal(F-B) photometric warp error
    * constrains the scene depth and camera motion to be in a common real world scale
    * improve photometric warp loss by considering a warp of deep features

## Introduction
* Contributions
    * an unsupervised framework for jointly learning a depth estimator and visual odometry estimator that does not suffer from the scale ambiguity
    * takes advantages of the full set of constraints available from spatial and temporal image pairs to improve upon prior art on single view depth estimations
    * produces the state-of-the-art frame-to-frame odometry results that are on par with geometric methods
    * uses a novel feature reconstruction loss in addition to the color intensity based image reconstruction loss which improves the depth and odometry estimation accuracy significantly.

## Method
Framework shown in Figure 2.

### Image reconstruction as a supervision
 we propose a stereo framework which constrains the scene depth and relative camera motion to be in a common, real-world scale, given an extra constraint set by **the known stereo baseline**.

* Temporal pair $(I_{L,t1}, I_{L, t2})$
* Spaital pair $(I_{L,t2}, I_{R, t2})$
* Reference view $I_{L, t2}$

We can synthesize two reference views, $I'_{L,t1}$ and $I'_{R,t2}$ from $I_{L,t1}$ and $I_{R,t2}$, respectively.

* $I'_{L,t1} = f(I_{L,t1}, K, T_{t2\rightarrow t1}, D_{L,t2})$
* $I'_{R,t1} = f(I_{R,t2}, K, T_{L\rightarrow R}, D_{L,t2})$
* $I_{L,t2} \rightarrow CNN_D \rightarrow D_{L,t2}$
* $(I_{L,t1}, I_{L,t2}) \rightarrow CNN_{VO} \rightarrow T_{t2 \rightarrow t1}$

The image reconstruction loss between the synthesized views and the real views

$L_{ir} = \sum_p (|I_{L,t2}(p) - I'_{L,t1}(p)| + |I_{L,t2}(p)-I'_{R,t2}(p)|)$.


### Differentiable geometry modules
The projected coordinates
* $p_{R,t_2} = K T_{L \rightarrow R} D_{L,t_2} (p_L, t_2) K^{-1} p_L, t_2$
* $p_{L,t_1} = K T_{t_2 \rightarrow t_1} D_{L,t_2} (p_L, t_2) K^{-1} p_L, t_2$

### Feature reconstruction as supervision
instead of using 3-channel color intensity information solely, we explore the use of dense features as an additional supervision signal.

* $F_{L,t_2}$, $F_{L,t_1}$, and $F_{R,t_2}$ corresponding dense feature representations

Then, the feature reconstruction loss can be formulated as,

$L_{fr} = \sum_p |F_{L,t_2}(p) - F'_{L,t_1}(p)| + \sum_p |F_{L,t_2}(p) - F'_{R,t_2}(p)|$

In this work, four possible dense features are considered.

### Edge-aware depth smoothness term

$L_{ds} = \sum^{W,H}_{m,n} | \partial_x D_{m,n}| e^{-|\partial_x I_{m,n}|} + |\partial_y D_{m,n}|e^{-|\partial_y I_{m,n}|}$


### Final loss

$L = \lambda_{ir} L_{ir} + \lambda_{fr} L_{fr} + \lambda_{ds} L_{ds}$


---

# Mahjourian et al., Unsupervised Learning of Depth and Ego-Motion from Monocular Video Using 3D Geometric Constraints, CVPR 2018

## Abstract
* Our main contribution is to explicitly consider the inferred 3d geometry of the scene, enforcing consistency of the estimated 3d point clouds and ego-motion across consecutive frames.
* We combine this novel 3d-based loss with 2d losses based on photometric quality of frame reconstructions using estimated depth and ego-motion from adjacent frames.
* we also incorporate validity masks to avoid penalizing areas in which no useful information exists.

## Introduction
This paper proposes a method for unsupervised learning of depth and ego-motion from monocular videos.

Given two consecutive frames from the video, a neural network produces single-view depth estimates from each frame and an ego-motion estimate from the frame pair.

Main contributions

* Imposing 3D constraints
    * we propose a loss which directly penalizes inconsistencies in the estimated depth without relying on backpropagation through the image reconstruction process.
* Principled masking
    * parallax effects, objects left/entered
    * computing the masks analytically leaves a simpler learning problem for the model
* Learning from an uncalibrated video stream
    * our approach can consume and learn from any monocular video source with camera motion.

## Method
Unlike 2D losses that enforce local photometric consistency, the 3D loss considers the entire scene and its geometry.

### Problem Geometry
At the training time, the goal is to learn depth & ego-motion from a single monocular video stream

* Given a pair of consecutive frames $X_{t-1}$ and $X_t$, estimate depth $D_{t-1}$, $D_t$ and the ego-motion $T_t$ representing the camera's movement (position and orientation) from time $t-1$ to $t$.

Once a depth $D_t$ is available, it can be projected into a point cloud $Q_t$

$Q^{ij}_{t} = D^{ij}_t \cdot K^{-1}[i, j, 1]^T$

Given $T_t$, $Q_t$ can be transformed to get an previous frame's point cloud

$\hat{Q}_{t-1} = T_t Q_t$
note that the transformation applied to the point cloud is the inverse of the camera movement from $t$ to $t-1$.

$\hat{Q}_{t-1}$ can be projected onto the camera at frame $t-1$ as $K \hat{Q}_{t-1}$

a mapping from image coordinates at time t to image coordinates at time $t-1$

$\hat{X}^{ij}_t = \hat{X}^{\hat{i}\hat{j}}_{t-1}$\
$[\hat{i}, \hat{j}, 1]^T = K T_t (D^{ij}_t \cdot K^{-1} [i, j, 1]^T)$

This process is repeated in the other direction to project $D_{t-1}$ into a point cloud $Q_{t-1}$ and reconstruct frame $\hat{X}_{t-1}$ by warping $X_t$ based on $D_{t-1}$ and $T^{-1}_t$

### Principled Masks
Fig. 3 demonstrates, validity masks can be computed analytically from depth an ego-motion estimates.
For every pair of frames $X_{t-1}$, $X_t$, one can create a pair of masks $M_{t-1}$, $M_t$ which can indicate the pixel coordinates where $\hat{X}_{t-1}$ and $\hat{X}_t$ are valid.

**nahyup** so no dynamics object handling...

### Image Reconstruction Loss
Photometric  consistency

$L_{rec} = \sum_{ij} ||(X^{ij}_t - \hat{X}^{ij}_t) M^{ij}_t||$

$\hat{X}_t$ is an approximation - because differentiability required so it is relatively crude

### A 3D Point Cloud Alignment Loss
We construct a loss function that directly compares point clouds $\hat{Q}_{t-1}$ to $Q_{t-1}$ or $\hat{Q}_{t}$ to $Q_{t}$.
This 3D loss uses a well-known rigid registration method, Iterative Closest Point(ICP) which computes a transformation that minimizes point-to-point distances between corresponding points in the two point clouds.

Our loss function uses both the computed transformation and the final residual registration error after ICP's minimization.

Because of the combinatorial nature of the correspondence computation, ICP is not differentiable.

More specifically, we use $T'_t$ as an approximation to the negative gradient of the loss with respect to the ego-moetion $T_t$. We therefore use $r_t$ as an approximation to the negative gradient of the loss with respect to the depth $D_t$


The complete 3D loss

$L_{3D} = ||T'_t - I||_1 + ||r_t||_1$

### Additional Image-based Losses
We use SSIM as a loss term in the training process. It measures the similarity between two images patches x and y

$SSIM(x,y)= \frac{(2\mu_x\mu_y+c_1)(2\sigma_{xy}+c_2)}{(\mu^2_x+\mu^2_y+c_1)(\sigma_x + \sigma_y + c_2)}$

where $\mu_x$ and $\sigma_x$ are the local means and variances.
$c_1=0.01^2$ and $c_2=0.03^2$

Since SSIM is upper bounded to 1 and needs to be maximized, we instead minimize

$L_{SSIM}=\sum_{ij}[1-SSIM(\hat{X}^{ij}_t, X^{ij}_t)]M^{ij}_t$

A depth smoothness loss is also employed to regularize the depth estimates.
We use a depth gradient smoothness loss that takes into account the gradients of the corresponding input images:

$L_{sm} = \sum_{i,j} || \partial_x D^{ij} || e^{-||\partial_x X^{ij}||} + || \partial_y D^{ij} || e^{-||\partial_y X^{ij}||}$


### Learning Setup
All loss functions are applied at four different scales:

$L=\sum_s \alpha L^s{rec} + \beta L^s_{3D} + \gamma L^s_{sm} +\omega L^s_{SSIM}$

we set to $\alpha=0.85$, $\beta=0.1$, $\gamma=0.05$, and $\omega=0.15$


---

# Li et al., UnDeepVO: Monocular Visual Odometry through Unsupervised Deep Learning, ICRA 2018

## Abstract
A novel monocular visual odometry(VO) system called UnDeepVO.
UnDeepVO is able to estimate the 6-DoF pose of a monocular camera and the depth of its view by using deep neural networks.

Two salient features:

* unsupervised deep learning scheme
* the absolute scale recovery

Specifically, we train UnDeepVO by using stereo image pairs to recover the scale but test it by using consecutive monocular images.

## Introduction


Our main contributions:

* We demonstrate a monocular VO system with recovered absolute scale, and we achieve this in an unsupervised manner by harnessing both spatial and temporal geometric constraints.
* Not only estimated pose but also estimated dense depth map are generated with absolute scales thanks to the use of stereo image pairs during training.
* We evaluate our VO system using KITTI dataset, and the results show UnDeepVO achieves good performance in pose estimation for monocular cameras.

## System Overview
Our system is composed of a pose estimator and a depth estimator.

Pose estimator

* VGG-based
* takes two consecutive monocular images as input
* predicts the 6-DoF transformation between them
* decouples the translation and the rotation with two separate groups of fully-connected layers after the last convolutional layer.
    * enables to introduce a weight normalizing the rotation and the translation predictions for better performance.

Depth estimator

* encoder-decoder archietecture to generate dense depth maps
* directly predict depth map (not disparity map)
    * whole system is easier to converge when training in this way

## Objective Losses for Unsupervised Training
The spatial image losses drive the network to recover scaled depth maps by using stereo image pairs,
while the temporal image losses are designed to minimize the errors on camera motion by using two consecutive monocular images.

### Spatial Image Losses of a Stereo Image Pair
The spatial losses are constructed from the geometric constraints between left and right stereo images.

* left-right consistenty loss
* disparity consistency loss
* pose consistency loss

#### Photometric Consistency Loss:
For the overlapped area between two stereo images, every pixel in one image can find its correspondence in the other with horizontal distance $D_p$.

Assume $p_l(u_l, v_l)$ is a pixel in the left image and $p_r(u_r, v_r)$ is its corresponding pixel in the right image.

Then, we have the spatial constraint $u_l = u_r$ and $v_l=v_r + D_p$

$D_p = B \frac{f}{D_{dep}}$

where $B$ is the baseline of the stereo camera, $f$ is the focal length and $D_{dep}$ is the depth value of the corresponding pixel.

With this spatial constraint and the calculated $D_p$ map, we could synthesize one image from the other through "spatial transformer". The combination of an L1 norm and an SSIM term is used to construct the left-right photometric consistency loss:

$L^l_{pho} = \lambda_s L^{SSIM} (I_l, I'_l) + (1-\lambda_s)L^{l_1}(I_l, I'_l)$
$L^r_{pho} = \lambda_s L^{SSIM} (I_r, I'_r) + (1-\lambda_s)L^{l_1}(I_r, I'_r)$

where $I_l$, $I_r$ are the original left and right images respectively.
$I'_l$ is the synthesized left image from the original right image $I_r$ and vice versa.

L^{SSIM} is the operation defined in [21]

[21] Z. Wang et al, Image quality assesment: From error visibility to structural similarity, IEEE TIP, vol.13, no.4, pp.600-612, 2004

#### Disparity Consistency Loss:
The disparity map UnDeepVO used is:

$D_{dis} = D_p \times I_W$

where $I_W$ is the width of original image size.
We can use $D_p$ to synthesize $D^{l'}_{dis}$, $D^{r'}_{dis}$ from $D^r_{dis}$, $D^l_{dis}$.

Then, the disparity consistency losses are

$L^l_{dis} = L^{l_1}(D^l_{dis}, D^{l'}_{dis})$
$L^r_{dis} = L^{l_1}(D^r_{dis}, D^{r'}_{dis})$

#### Pose Consistency Loss:
We use both left and right image sequences to predict the 6-DoF transformation of the camera separately during training.

Ideally, these two predicted transformations should be basically identical.

$L_{pos} = \lambda_p L^{l_1}(\mathbf{x}'_l, \mathbf{x}'_r) + \lambda_o L^{l_1}(\phi'_l, \phi'_r)$

where $[\mathbf{x}'_l, \phi'_l]$ is the predicted poses from the left image sequences.

### Temporal Image Losses of Consecutive Monocular Images

Essential part to recover the 6-DoF motion of camera
It comprises photometric consistency loss and 3D geometric registration loss.

#### Photometric Consistency Loss:

$p_{k+1} = K T_{k, k+1} D_{dep} K^{-1} p_k$

We can synthesize $I'_k$ and $I'_{k+1}$ from $I_{k+1}$ and $I_k$ by using estimated poses and spatial transformer.

Therefore, the photometric losses between the monocular image sequence are

$L^k_{pho}=\lambda L^{SSIM}(I_k, I'_k)+(1-\lambda_s)L^{l_1}_(I_k, I'_k)$\
$L^{k+1}_{pho}=\lambda L^{SSIM}(I_{k+1}, I'_{k+1})+(1-\lambda_s)L^{l_1}_(I_{k+1}, I'_{k+1})$

#### 3D Geometric Registration Loss:
3D geometric registration loss is to estimate the transformation by aligning two point clouds, similar to the ICP.

Assume $P_k(x,y,z)$ is a point in the k-th camera coordination.
Then, the 3D geometric registration losses between two monocular images are

$L^k_{geo} = L^{l_1}(P_k, P'_k)$\
$L^{k+1}_{geo} = L^{l_1}(P_{k+1}, P'_{k+1})$

**nahyup** is it L1 norm of point coordinate itself not image value?