# Tips on weight reusing

## Issues
We often use convolutional neural networks reuse to see the feature variation according to the input variation.
Usually, we call this 'weighting sharing' or 'multi-forwarded'

To do such tasks, I think that we need to carefully treat the neural network especially when the neural network has batch normalization layer.

~~~ python
import torch
l = torch.randn(1, 3, 256, 512)
r = torch.randn(1, 3, 256, 512)
l_feat = net(l)
r_feat = net(r)
~~~

What input l is selected, will affect the 'r_feat', because the BN layer changes not even call backward due to its moving mean/std tracking nature.
It will make training process instable.


## Tips

### Make a batch, call the network once then sparate

I think that this is the best way to avoid issues discribed above.

If we call same neural network several times with different input tensors, first, make it as a one tensor

~~~ python
import torch
l = torch.randn(1, 3, 256, 512)
r = torch.randn(1, 3, 256, 512)
output = net(torch.cat((l, r), dim=0))
l_feat = output[0]
r_feat = output[1]
~~~
### Remove batch norm layers

This is a simple trick.
But, you should care that to add bias term that is usally removed when using batchnorm

~~~ python
import torch
net_org = torch.nn.Sequential(
            torch.nn.Conv2d(3, 16, 3, 1, 1, bias=False),
            torch.nn.Batchnorm2D(16),
            torch.nn.ReLU())

net = torch.nn.Sequential(
            torch.nn.Conv2d(3, 16, 3, 1, 1),
            torch.nn.ReLU())

l = torch.randn(1, 3, 256, 512)
r = torch.randn(1, 3, 256, 512)
l_feat = net(l)
r_feat = net(r)
~~~

## Experimental results on the self-supervised scene depth and pose estimation

all evaluation test on KITTI '07' sequence

### Baseline

| db type      | abs trans    | abs rot.     | rel trans    | rel rot.     |
| ---          | ---          | ---          | ---          | ---          |
| '07'         | 
| '04-11'      | 2.407599e+01 | 2.498723e+00 | 1.231157e+00 | 2.361589e-01 |
| '04-11'-'07' | 


### Make a batch

| db type      | abs trans    | abs rot.     | rel trans    | rel rot.     |
| ---          | ---          | ---          | ---          | ---          |
| '07'         | 3.323901e+01 | 1.291120e+01 | 1.008487e+00 | 1.249435e+00 |
| '04-11'      | 7.402273e+00 | 2.101689e+00 | 9.503826e-02 | 1.512228e-01 |
| '04-11'-'07' | 3.469390e+01 | 8.046043e+00 | 1.143741e+00 | 9.973068e-01 |

### Remove BN

| db type      | abs trans    | abs rot.     | rel trans    | rel rot.     |
| ---          | ---          | ---          | ---          | ---          |
| '07'         | 1.308426e+02 | 8.308176e+01 | 6.764839e+00 | 1.035058e+01 |
| '04-11'      | 7.184021e+00 | 2.751324e+00 | 1.036151e-01 | 1.510322e-01 |
| '04-11'-'07' | 3.130275e+01 | 1.001634e+01 | 1.063457e+00 | 1.099961e+00 |


## What happen when we use batch-trick and add 3frame loss

### Make a batch with 3frames training loss

| db type      | abs trans    | abs rot.     | rel trans    | rel rot.     |
| ---          | ---          | ---          | ---          | ---          |
| '07'         | 1.340637e+01 | 5.177149e+00 | 1.911524e-01 | 3.870371e-01 |
| '04-11'      | 2.556447e+00 | 4.780668e-01 | 9.336385e-02 | 1.854351e-01 |
| '04-11'-'07' | 2.668269e+01 | 1.044389e+01 | 7.484354e-01 | 9.903502e-01 |
