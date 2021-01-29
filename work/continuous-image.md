# Idea note on continuous representation of image

## Overview

* Stage 1
  * Spatial Resolution $\rightarrow$ Super-resolution
* Stage 2
  * Temporal Resolution $\rightarrow$ Video Upsampling, Motion Blur
* Concept
  * Computational/Neural Modeling of Sensor
  * By Coupling degradation(Blur) kernel

## Continuous Image Representation

in other words, image regression

### Discrete Image Representation

Let $i \in {0, \cdots, W-1}$ and $j \in {0, \cdots, H-1}$,

$\bar{I}(i,j) = c$

where, $c \in \mathbb{R}^3$ i.e. RGB

### Continuous Image Representation

Let $(x,y) \in [0,1] \times [0,1]$,

$I(x,y) = c$

### The relationship between Discrete and Continuous Representation

$\bar{I}(i,j) = I(\lfloor i/W \rfloor, \lfloor j/H \rfloor)$

In here, we assume that the discree image is sampled by dirac-delta function

In practice, there are some kernel functions that reflect blur or noise when sampling from the continuous image.


## Why continous representation?

The most of problems of image and video processing can be treated as a sampling problem.

* Super-Resolution

* Video Frame Upsampling

* Motion Deblurr

If we can find the continuous representation of a given image, the above problems will be solved trivially.


## How?

We will define a continuous representation of a image as a MLP (Multi-layer Perceptron).

i.e.,

$(x,y) \rightarrow MLP \rightarrow (r,g,b)$

## Really working? There are promissing examples

* DeepSDF
* NeRF
* Continuous Image Representation

## Challenging Issues

* Positional Encoding & High-Frequency
* One MLP for one image?
* how to extend to temporal domain. i.e., Continuous Representation of Video?
* How to define continuous kernel function to model the degradation (such as optical blur) and noise.
