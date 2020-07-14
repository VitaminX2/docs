# Mathematical Introduction to Computer Vision and Computer Graphics Camera

## Intrisic Camera Matrix

$K = \begin{pmatrix} fx & 0 & cx \\ 0 & fy & cy \\ 0 & 0 & 1 \end{pmatrix}$

The corresponding 3D coordinate is $[x, y, z]^T = D K^{-1} [u, v, 1]^{T}$

If we scale and/or crop the image, using matrix $M$,


$S = \begin{pmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{pmatrix}^{-1}$
$C = \begin{pmatrix} 1 & 0 & x_{min} \\ 0 & 1 & y_{min} \\ 0 & 0 & 1 \end{pmatrix}^{-1}$

$M = SC$

$M^{-1} = \begin{pmatrix} 1 & 0 & x_{min} \\ 0 & 1 & y_{min} \\ 0 & 0 & 1 \end{pmatrix} \begin{pmatrix} s_x & 0 & 0 \\ 0 & s_y & 0 \\ 0 & 0 & 1 \end{pmatrix}$

Then, the modified 3d coordinate is $[x, y, z]^T = D K^{-1} M^{-1} [u', v', 1]^{T}$, where $u'$ and $v'$ is the coordinate from the modified image.

We may write,

$K' = MK = \begin{pmatrix} \frac{1}{s_x} & 0 & -\frac{x_{min}}{s_x} \\ 0 & \frac{1}{s_y} & -\frac{y_{min}}{s_y} \\ 0 & 0 & 1 \end{pmatrix}\begin{pmatrix} fx & 0 & cx \\ 0 & fy & cy \\ 0 & 0 & 1 \end{pmatrix}$

### Matching Intrinsic Camera Matrices

Let assume that we have two different image sets, such that the intrinsic matrices and image sizes are given as $K_A, K_B$ and $W_A \times H_A$, $W_B \times H_B$, respectively.

Let's find the resizing and crop parameters to meet the camera intrinsic matrices to be the same


$K' = \begin{pmatrix} \frac{1}{s_x} & 0 & -\frac{x_{min}}{s_x} \\ 0 & \frac{1}{s_y} & -\frac{y_{min}}{s_y} \\ 0 & 0 & 1 \end{pmatrix}\begin{pmatrix} fx & 0 & cx \\ 0 & fy & cy \\ 0 & 0 & 1 \end{pmatrix}$

$\begin{pmatrix} {f_x}\prime & 0 & c_x\prime \\ 0 & f_y\prime & c_y\prime \\ 0 & 0 & 1 \end{pmatrix} = \begin{pmatrix} \frac{f_x}{s_x} & 0 & \frac{c_x - x_{min}}{s_x} \\ 0 & \frac{f_y}{s_y} & \frac{c_y - y_{min}}{s_y} \\ 0 & 0 & 1 \end{pmatrix}$

Then, the final scaling and croping parameters are:

$s_x = \frac{f_x}{f_x\prime}$

$s_y = \frac{f_y}{f_y\prime}$

$x_{min} = c_x - s_x c_x\prime$

$y_{min} = c_y - s_y c_y\prime$

## Basic CV operations on Camera

$[u, v, 1]^{T} = K * [R|t] * [X, Y, Z, 1]^{T}$

## Key difference between opengl and opencv coordinates
* OpenGL has inverted Y & Z axes relative to OpenCV.

$MV =  \begin{pmatrix} 1 & 0 & 0 & 0\\ 0 & -1 & 0 & 0\\ 0 & 0 & -1 & 0\\ 0 & 0& 0 & 1 \end{pmatrix} \cdot \begin{pmatrix} r00 & r01 & r02 & t1\\ r10 & r11 & r12 & t2\\ r20 & r21 & r22 & t3\\ 0 & 0 & 0 & 1 \end{pmatrix}$

## Projection matrix
* OpenCV projection matrix is 3x3 and doesn't consider the z-order culling that an OpenGL system does.
* scale the output of the projection matrix to be in the [-1, 1] (normalized device coordinates)
* also have to scale it on the incoming side as the OpenCV projectio matrix will go back to the size of the input image.


$P = \begin{pmatrix} 0 & 1 & 0 & 0 \\ -1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{pmatrix}\begin{pmatrix} a00 & 0 & a02 & 0 \\ 0 & a11 & a12 & 0 \\ 0 & 0 & a22 & a23 \\ 0 & 0 & -1 & 0 \end{pmatrix}$

$a00 = 2K_{00}/w$

$a02 = -1 + (2K_{02})/w$

$a11 = 2K_{11}/h$

$a12 = -1 + 2K_{12}/h$

$a22 = -(far+near)/(far-near)$

$a23 = -2far\cdot near / (far-near)$

$K = \begin{pmatrix} fx & 0 & cx \\ 0 & fy & cy \\ 0 & 0 & 1 \end{pmatrix}$


## Overview of SE3 pose space

![SE(3) Pose Space](./img_kitti_eval/pose_space.png)

* Pose composition: $\mathbf{a} \equiv \mathbf{P} \oplus \mathbf{a'}$
* Pose inverse composition: $\mathbf{a'} \equiv \mathbf{a} \ominus \mathbf{P}$

In a matrix form,

* $\mathbf{M}$: 4x4 matrix corresponding to a 6D pose $\mathbf{P}$
* $\mathbf{a'}$: $[a'_x, a'_y, a'_z]$
* $\mathbf{a}$: $[a_x, a_y, a_z]$

Then, above pose composition and inverse composition can be written as,

* $a = P \oplus a'$ $\rightarrow$ $[a_x, a_y, a_z 1]^T = M [a'_x, a'_y, a'_z, 1]^T$
* $a' = a \ominus P$ $\rightarrow$ $[a'_x, a'_y, a'_z 1]^T = M^{-1} [a_x, a_y, a_z, 1]^T$

And, SE(3) matrix can be inversed as follows

* $M = \begin{bmatrix} R & t \\ 0 & 1 \end{bmatrix}$
* $M^{-1} = \begin{bmatrix} R^{T} & -R^Tt \\ 0 & 1 \end{bmatrix}$

Composition of two poses $M_1$ and $M_2$

* $M = M_1 M_2$

Relative pose between two poses $M_{prev}$ and $M_{curr}$

* $M_{curr} = M_{prev} M_{rel}$
* $M_{rel} = M_{prev}^{-1} M_{curr}$

## My implementation 
* The obtained 6DoF represent R,T that transforms src points to dst points not transform src camera to dst camera.
* It means that if a R,T which is $M$, is obtained the camera transform is $M^{-1}$
* If you want combine two subsequent $R_i$, $T_i$ and $R_j$, $T_j$
    * $M_i = \begin{bmatrix} R_i & t_i \\ 0 & 1 \end{bmatrix}$
    * $M^{-1}_i = \begin{bmatrix} R^{T}_i & -R^{T}_i t_i \\ 0 & 1 \end{bmatrix}$
    * $M_j = \begin{bmatrix} R_j & t_j \\ 0 & 1 \end{bmatrix}$
    * $M^{-1}_j = \begin{bmatrix} R^{T}_j & -R^{T}_j t_j \\ 0 & 1 \end{bmatrix}$
* The composed comera tranform is $M^{-1}_i M^{-1}_j = \begin{bmatrix} R^{T}_i & -R^{T}_i t_i \\ 0 & 1 \end{bmatrix}\begin{bmatrix} R^{T}_j & -R^{T}_j t_j \\ 0 & 1 \end{bmatrix} = \begin{bmatrix} R^{T}_i R^{T}_j & -R^{T}_i R^{T}_j t_j - R^{T}_i t_i \\ 0 & 1 \end{bmatrix}$
* Then the composed R, T is
    * $R = (R_i^T R_j^T)^T = R_j R_i$
    * $t = -R (-R_i^TR_j^T t_j - R_i^T t_i) = R_j R_i (R_i^T R_j^T t_j + R_i^T t_i) = t_j + R_j t_i$
