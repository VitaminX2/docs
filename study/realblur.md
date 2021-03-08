# Some quote on RealBlurDB

## Image Acquisition System and Process

### Image Acquisition System

The estimated baseline is 8.22 mm, which corresponds to disparity of less than four pixels for objects more than 7.8 meters
away in the full resolution, and less than one pixel in our final dataset, which contains images downsampled by 1/4.

### Image Acquisition Process

For each scene, we first captured a pair of two sharp images, referred to as a reference pair, which will be used for geometric and photometric
alignment of sharp and blurred images in the postprocessing step

We then, captured 20 pairs of blurred and sharp images of the same scene to increase the amount of images and the diversity of camera shakes

## Postprocessing

As we use a beam splitter and an optical enclosure as well as wide-angle lenses,
images have invalid areas along the boundaries that capture outside the beam
splitter or inside the optical enclosure. Thus, we crop out such regions

We then correct lens distortions in the cropped images using distortion parameters
estimated in a separate calibration step [15].

Then, we downsample the images, and perform denoising to the downsampled sharp image

Finally, we perform geometric and photometric alignment.

The sizes of the images from the cameras, after cropping, and after downsampling are 7952 ×5304, 2721×3094, and 680×773

### Downsampling & Denoising

three purposes

* First, while the image resolutions of recent cameras are very high, even the latest deep learning-based deblurring methods cannot handle such high-resolution images. 
* Second, as we use high ISO values to capture sharp images, they have amplified noise, which can adversely affect training and evaluation of deblurring methods using the sharp images. Downsampling can reduce such noise as it averages nearby pixel intensities. 
* Third, as the alignment of the cameras in our image acquisition system is not perfect, there can exist a small amount of parallax between sharp and blurred images, which can also be effectively reduced by downsampling

### Geometric Alignment

There still exists some amount of geometric misalignment (Fig. 5(a)).
Furthermore, the positions of the cameras may slightly change over time due to camera shakes.

In the first step, we roughly align each blurred and sharp image pair using
a homography. For homography
estimation, we use the enhanced correlation coefficients method [11]

Even after alignment using a homography, there can still exist minuscule
misalignment between blurred and sharp images due to their different shutter
speeds. (이건 sharp reference로 맞춰도 안맞음)

in the second step, we estimate the remaining misalignment between
each pair of blurred and sharp images, and align them. To this end, we use a
phase correlation-based approach [35] that can robustly estimate a similarity
transform under the presence of blur 

The phase correlation-based alignment, however, cannot align the contents
in the blurred and sharp images with respect to their centers. Thus, in the third
step, we align the blurred and sharp images to match their centers.
we align images to match the center of an object in a sharp image with the center of its
corresponding object in a blurred image in an additional alignment step for each
pair of blurry and sharp images.

### Photometric Alignment

Although we use cameras and lenses of the same models, their images may have slight intensity difference.
Specifically, for geometrically aligned sharp and blurred images s and b, we photometrically align s to b by applying a linear transform αs + β so that αs + β ≈ b.
we estimate them from the reference pair corresponding to s and b.
Specifically, α and β are estimated as α = σ1=σ2 and β = µ1 − αµ2 where σ1 and σ2 are the standard deviations of the reference images, and µ1 and µ2 are their means

## Refererences

11. Evangelidis, G.D., Psarakis, E.Z.: Parametric image alignment using enhanced correlation coefficient maximization. TPAMI 30(10), 1858{1865 (Oct 2008)

15. Heikkila, J., Silven, O.: A four-step camera calibration procedure with implicit image correction. In: CVPR. p. 1106. CVPR ’97, IEEE Computer Society, USA (1997)
