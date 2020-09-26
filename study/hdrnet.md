# Survey on HDRnet related works
● ▶□
## Real-time MC Denoising with the Neural Bilateral Grid, EGSR 2020

### Introduction

Different from Gharbi et al. [GCB∗17], we embed the noisy radiance data in the bilateral grid by accumulating multiple neighboring pixels into a specific grid element, and slice directly from the grid to get the denoised data. 
Our approach avoids the explicit computation of per-pixel weights for large kernels and achieves high-quality denoising with fast, spatially uniform filters.

### Method

we design a neural bilateral grid for denoising Monte Carlo renderings.

#### Step.1 Creating a bilateral grid Mi

A bilateral grid Mi is a three dimensional embedding of an image, which we can write as Mi : R3 → R3, where the input is a location in 3D space, and the output is a radiance value.

$r_i(j)$ the j-th pixel in a noisy input frame

$M_i(p) = \Phi \{r_i | g_i\}(p) = \sum_j r_i(j) \delta (g_i (j) - p)$

where $p \in R^3$ is a spatial location,
$\delta$ is a Dirac impulse,
$g_i(j) \in R^3$ is a guide

#### Step.2 Applying 3D convolution to filter noise

We apply a three-dimensional filter F to Mi to achieve a specific image processing task, that is, noise removal from Monte Carlo renderings in our application.

$M'_i(p) = M_i \circledast F(p) = \sum_j F(g_i(j) - p) r_i(j)$

#### Step.3 Reconstructing frame i by extracting data from the grid M' i.e., slicing

$R_i(j) = M'_i(g_i(j))$

$R_i(j) = \{ \Phi \{r_i | g_i \} \circledast F \} (g_i (j))$




## Fast Image Processing with fully convolutional networks
