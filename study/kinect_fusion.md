# KinectFusion
* KinectFusion: Real-Time Dense Surface Mapping and Tracking

# Abstract
* Fuse all of the depth data streamed from a kinect sensor into a single global implicit surface model
* The current sensor pose is simultaneously obtained by tracking the live depth frame relative to the global model using a coarse-to-fine ICP algorithm

# Introduction
* real-time dense volumetric reconstruction 
* track 6DoF pose of the sensor
* tracking 30Hz frame-rate relative to the fully up-to-date fused dense model

# Background
## Kinect Sensor
* 640x480 11bit 30Hz

## Drift-Free SLAM for AR
* Tracking: Estimating the pose of the sensor on the assumption 
* Mapping: improving and expanding the map using a form of global opimization

# Method
## Overview
### Surface measurement
* A pre-processing stage where a dense vertex map and normal map pyramid are generated from the raw depth measurements obtained from the kinect device.

### Surface reconstruction update
* The global scene fusion
* given the pose determined by tracking the depth data from a new sensor frame,
* surface measurement is integrated into the scene model maintained with a volumetric, truncated signed distance function rep.

### Surface prediction
* close the loop between mapping and the localization by tracking the live depth frame against the globally fused model.
* raycasting the signed distance function into the estimated frame to provice a dense surface prediction against which the live depth map is aligned 

### Sensor pose estimation
* Live sensor tracking is achieved using a multi-scale ICP alignment between predicted surface and current sensor measurement.

## Preliminaries
* 6DoF camera pose at time t by a rigid body transformation matrix
$T_{g,k} = \begin{bmatrix} R_{g,k} & \mathbf{t}_{g,k} \\ \mathbf{0}^{T} & 1 \end{bmatrix} \in \mathbb{SE}_3$
* this maps camera coordinate frame at time k into the global frame g

$\mathbf{p}_g = T_{g,k}\mathbf{p}_k$

* a single constant camera calibration matrix K

$\mathbf{q} = \pi(\mathbf{p})$
$\mathbf{p} \in \mathbb{R}^3 = (x,y,z)^{T}$

## Surface Measurement
* time k, a raw depth map $R_k$

$\mathbf{p}_k = R_k(u)K^{-1}\dot{\mathbf{u}}$

is a metric point measurement in the sensor frame of reference $k$

* apply bilateral filter to the raw depth map to obtain a discontinuity preserved depth map with reduced noise $D_k$

* back project the filtered depth values into the sensor's frame of reference to obtain a vertex map $\mathbf{V}_k$

$\mathbf{V}_k(u) = D_k(\mathbf{u})\mathbf{K}^{-1}\dot{\mathbf{u}}$

* 3D point는 unproject하고 Depth만큼 Scale하면 얻어진다...

* normal vector는 Neighbor cross product로 구할 수 있다.

$\mathbf{N}_k(u)=\nu [(\mathbf{V}_k(u+1, v)-\mathbf{V}_k(u,v)) \times (\mathbf{V}_k(u,v+1)-\mathbf{V}_k(u,v))]$
where $v[x]=\mathbf{x}/||\mathbf{x}||_2$

* vertex validity mask
* $M_k(\mathbf{u}) \mapsto 1$ if a depth measurement transforms to a valid vertex
* $M_k(\mathbf{u}) \mapsto 0$ otherwise 

* L=3 level multi-scale representation of the surface measurement
* a depth map pyramid $D^{l\in[1...L]}$
* depth values are used in the average only if they are within $3\sigma_r$ of the central pixel to ensure smoothing does not occur over depth boundaries
* subsequently, $\mathbf{V}^{l in [1..L]}$, $\mathbf{N}^{l\in[1..L]}$

* the global frame

$\mathbf{V}^g_k(\mathbf{u})=T_{g,k}\dot{\mathbf{V}}_k(\mathbf{u})$

$\mathbf{N}^g_k(\mathbf{u})=R_{g,k}\mathbf{N}_k(\mathbf{u})$

## Mapping as Surface Reconstruction
* a fusion of the registered depth measurements

$\mathbf{S}_k(\mathbf{p}) \mapsto [F_k(\mathbf{p}), W_k(\mathbf(p))]$


* two important contraints
    * truncate the uncertainty of a depth measurement such that the true value lies within $\mu$, then a distance r from the camera center along each depth map ray, $r\lt (\lambda R_k(\mathbf{u})-\mu))$, here $\lambda=||K^{-1}\dot{\mathbf{u}}||_2$ scales the measurement along the pixel ray.
    (요기는 빈공간)
    * No surface information is obtained in the reconstruction volume at $r\gt(\lambda R_k(\mathbf{u})+\mu)$ along the camera ray
    (요기는 정보가 없는 공간)

$|r-\lambda R_k(\mathbf{u})|\leq\mu$


* $R_k$, $T_{g,k}$ 있을때, global frame의 점 $\mathbf{p}$에서 $TSDF[F_{R_k}, W_{R_k}]$ 계산은,

$F_{R_k}(\mathbf{p}) = \Psi(\lambda^{-1} ||\mathbf{t}_{g,k}-\mathbf{p}||_2 - R_k(\mathbf{x}))$
* $\mathbf{t}_{g,k}$는 카메라 위치로 보인다.

$\lambda = ||K^{-1} \dot{\mathbf{x}}||_2$
* $1/\lambda$ converts the ray distance to $\mathbf{p}$ to a depth

$\mathbf{x} = \lfloor \pi (KT^{-1}_{g,k}\mathbf{p})\rfloor$

$\Psi(\eta) = \begin{cases} min(1, \frac{\eta}{\mu} sgn(\eta) & iff \eta \geq -\mu \\ null & otherwise \end{cases}$

$W_{R_k}(\mathbf{p}) \propto cos(\theta)/R_k(\mathbf{x})$

$\min_{F\in\mathcal{F}} \sum_k ||W_{R_k}F_{R_k}-F)||_2$

* solution can be obtained incrementally as more data terms are added using a simple weighted running average

$F_k(\mathbf{p}) = \frac{W_{k-1}(\mathbf(p))F_{k-1}(\mathbf{p})+W_{R_k(\mathbf{p})F_{R_k(\mathbf(p))}}}{W_{k-1}(\mathbf{p})+W_{R_k(\mathbf{p})}}$

$W_k(\mathbf{p}) = W_{k-1}(\mathbf{p} + W_{R_k}(\mathbf{p})$

## Surface Prediction from Ray Casting the TSDF

$R_{g,k}\hat{\mathbf{N}}_k = \hat{\mathbf{N}}^g_k(\mathbf{u}) = \nu [\nabla F(\mathbf{p})]$
$\nabla F = [\frac{\partial F}{\partial x}, \frac{\partial F}{\partial y}, \frac{\partial F}{\partial z}]$
* normal

## Sensor Pose Estimation
Current sensor pose $T_{w,k} \in \mathbb{SE}_3$

by maintaining a high tracking frame-rate, we can assume small motion from one frame to the next.

This allow us

* to use the fast projective data association algorithm[4] to obtain correspondence and the point-plane metric[5] for pose optimisation

    * [4] G. Blais and M. D. Levine. Registering multiview range data to create 3D computer objects. IEEE Transactions on Pattern Analysis and Machine Intelligence (PAMI), 17(8):820–824, 1995. 2.3, 3.5
    * [5] Y. Chen and G. Medioni. Object modeling by registration of multiple range images. Image and Vision Computing (IVC), 10(3):145–155, 1992. 2.3, 3.5

* the data association and point-plane optimization can use all of the available surface measurements

We track the current sensor frame by aligning a live surface measurement $(\mathbf{V}_k, \mathbf{N}_k)$ against the model prediction from the previous frame $(\hat{\mathbf{V}}_{k-1}, \hat{\mathbf{N}}_{k-1})$.

The global point-plane energy, under the $\mathcal{L}_2$ norm for the desired camera pose estimate $T_{g,k}$

$\mathbf{E}(T_{g,k}) =$
$\sum_{\mathbf{u} \in \mathcal{u}, \omega_k(\mathbf{u}) \neq null}$
$|| \left(T_{g,k} \dot{\mathbf{V}}_k (\mathbf{u}) - \hat{\mathbf{V}}^g_{k-1} (\hat{\mathbf{u}})\right)^{T}$
$\hat{\mathbf{N}}^g_{k-1} (\hat{\mathbf{u}}) ||_2$

The projective data association alrogithm produces the set of vertex correspondences $\{\mathbf{V}_k, \hat{\mathbf{V}}_{k-1}(\hat{\mathbf{u}}) | \Omega (\mathbf{u}) \neq null\}$ by computing the perspectively projected point, $\hat{\mathbf{u}} = \pi (K \tilde{T}_{k-1,k} \dot{\mathbf{V}_k}(\mathbf{u}))$ using an estimate for the frame-frame transform $\tilde{T}^z_{k-1,k} = T^{-1}_{g,k-1} \tilde{T}^z_{g,k}$