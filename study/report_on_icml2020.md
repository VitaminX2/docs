# ICML 2020 학회 조사 결과

## Generative Models & Image Processing

강은희 전문님 작성

## Neural Scene Representation

Vincent Sitzmann이 NeurIPS 2019에 Scene Representation Networks를 발표하여, Honorable Mention "Outstanding New Directions" 를 받은 이래로,
다양한 연구들이 Neural Scene Representaion 분야에 발표되고 있다.
ICML 2020도 이러한 경향이 반영되고 있으며, Neural Scene Representation과 직접적으로 연관된 Surface와 Volume에 대한 Geometry Generation 논문 5편, Neural Rendering 연구 2편, 간접적으로 연관된 Topology 및 Dynamics 관련 연구도 5편 등 총 12편의 관련 연구가 발표되었다.

Geometric Reprentation 관점에서는 Gropp등은 DeepSDF (CVPR 2019)로 대표되는 Implicit Surface Representation 방식인 Signed Distance Function을 Neural Network로 Encoding할때, Partial Differential Equation인 Eikonal Equation으로 모델링되던 Distance Field Gradient에 대한 Constraint를 반영하여 더 Robust한 결과를 얻을 수 있음을 보였다. Implicit Surface 생성 외에도, Hypernetwork방식을 이용한 Point Cloud 생성 (Spurek 등) 방식과 Polygonal Mesh를 직접 생성해내는 방식들(Lei, Nash등)도 소개되었다.

Differentiable Rendering은 3D Geometric Model과 2D Image간의 Missing Link를 이어줄 수 있는 기술 분야로 많은 관심을 받고 있다.
ICML 2020에는 2편의 연구가 소개되었는데, Han등은 복잡한 Differentiable Renderer의 구성없이 Silhouette Image를 통한 Potential Field를 구성하여 Point Cloud Generation이 가능함을 보였다. 한편, Dupont등이 발표한 Equivariant Neural Rendering은 기존의 Convolution이 Translation equivarant함은 잘 알려져있으나, Rotation에 대해서는 간과되었음에 주목하여, Geometric Model이 Rotation등의 Transform이 발생하면, 대응되는 Neural Rendering Image도 동일한 Transform이 유지(Equivariant)되도록 하여, 높은 성능의 Neural Renderer를 학습할 수 있음을 보였다. Equivariance나 Symmetry는 ICML 2020 학회 전반에 걸쳐 중요한 화두로 다뤄지고 있고, 대표적으로 On Learning Sets of Symmetric Elements같은 논문이 Outstanding Paper Awards를 수상하기도 하였다.

Physics 또는 Dynamics Modeling 관련 연구도 다수 발표되었는데, 특히 Particle-based Fluid Dynamics를 Neural Network로 학습하여 가능하게 한 논문을 주목할만하다.
Sanchez등이 발표한 Learning to Simulate Complex Physics with Graph Networks로, 기존의 Fluid Motion을 모델링하는 Partial Differential Equation인 Navier-Stokes Equation을 Data-driven Neural Network로 학습하여 유사한 정확도를 보일 수 있음을 확인하였다. 

Ian Goodfellow가 Generative Adverarial Nets 를 NeurIPS 2014에 발표한 이래로, Computer Graphics 의 모든 Pipeline을 GAN이 대체할 것이라던 전망은 Fully Controllable GAN에 대한 요구로 이어졌고, 아직까지 연구가 활발히 되고 있다. Neural Scene Representation은 이러한 연구 방향의 연장선에서 Quality 관점에서는 3D Geometric Modeling과 Physics Modeling 분야에 상당히 Promissing한 연구가 가능할 것을 기대하게 한다.

### Awards

* Test of Time Award
  * Gaussian Process Optimization in the Bandit Setting: No Regret and Experimental Design
  * Niranjan Srinivas, Andreas Krause, Sham Kakade, Mathias Seeger
* Outstanding Paper Awards
  * On Learning Sets of Symmetric Elements
  * Tuning-free Plug-and-Play Poximal Algorithm for Inverse Imaging Problems
* Oustanding Paper
  * Efficiently sampling functions from Gaussian process posteriors
  * Generative Pretaining From Pixels

### Papers related with Generative Models & Image Processing

* Generative Pretraining From Pixels
  * Mark Chen, Alec Radford, Rewon Child, Jeffrey K Wu, Heewoo Jun, David Luan, Ilya Sutskever

### Papers related with Neural Scene Representation

#### Theory

* Topological Autoencoders
  * Michael Moor, Max Horn, Bastian Rieck, Karsten Borgwardt,

#### Geometry - Surfaces and Volumes

* Implicit Geometric Regularization for Learning Shapes
  * Amos Gropp, Lior Yariv, Niv Haim, Matan Atzmon, Yaron Lipman
* Hypernetwork approach to generating point clouds
  * Przemysław Spurek, Sebastian Winczowski, Jacek Tabor, Maciej Zamorski, Maciej Zieba, Tomasz Trzcinski
* Analytic Marching: An Analytic Meshing Solution from Deep Implicit Surface Networks
  * Jiabao Lei, Kui Jia
* PolyGen: An Autoregressive Generative Model of 3D Meshes
  * Charlie Nash, Yaroslav Ganin, S. M. Ali Eslami, Peter Battaglia
* Learning Algebraic Multigrid Using Graph Neural Networks
  * Ilay Luz, Meirav Galun, Haggai Maron, Ronen Basri, Irad Yavneh

#### Appearance - Rendering

* DRWR: A Differentiable Renderer without Rendering for Unsupervised 3D Structure Learning from Silhouette Images
  * Zhizhong Han, Chao Chen, Yu-Shen Liu, Matthias Zwicker
* Equivariant Neural Rendering
  * Emilien Dupont, Miguel Bautista Martin, Alex Colburn, Aditya Sankar, Joshua M Susskind, Qi Shan

#### Dynamics

* Learning Similarity Metrics for Numerical Simulations
  * Georg Kohl, Kiwon Um, Nils Thuerey,
* Learning to Simulate Complex Physics with Graph Networks
  * Alvaro Sanchez, Jonathan Godwin, Tobias Pfaff, Zhitao Ying, Jure Leskovec, Peter Battaglia,
* Visual Grounding of Learned Physical Models
  * Yunzhu Li, Toru Lin, Kexin Yi, Daniel Bear, Daniel Yamins, Jiajun Wu, Josh Tenenbaum, Antonio Torralba
* Combining Differentiable PDE Solvers and Graph Neural Networks for Fluid Flow Prediction
  * Filipe de Avila Belbute-Peres, Thomas Economon, Zico Kolter,

#### General Geometric

* All in the (Exponential) Family: Information Geometry and Thermodynamic Variational Inference
  * Rob Brekelmans, Vaden W Masrani, Frank Wood, Greg Ver Steeg, Aram Galstyan
* Understanding Contrastive Representation Learning through Geometry on the Hypersphere
  * Tongzhou Wang, Phillip Isola
* Stochastic Flows and Geometric Optimization on the Orthogonal Group
  * Krzysztof Choromanski, David Cheikhi, Jared Q Davis, Valerii Likhosherstov, Achille Nazaret, Achraf Bahamou, Xingyou Song, Mrugank Akarte, Jack Parker-Holder, Jacob Bergquist, YUAN GAO, Aldo Pacchiano, Tamas Sarlos, Adrian Weller, Vikas Sindhwani
* PackIt: A Virtual Environment for Geometric Planning
  * Ankit Goyal, Jia Deng
* Incidence Networks for Geometric Deep Learning
  * Marjan Albooyeh (UBC) · Daniele Bertolini (N/A) · Siamak Ravanbakhsh (McGill - Mila)
* A Geometric Approach to Archetypal Analysis via Sparse Projections
  * Vinayak Abrol (Mathematical Institute Oxford) · Pulkit Sharma (Quantum Black)
* Knowing The What But Not The Where in Bayesian Optimization
  * Vu Nguyen (University of Oxford) · Michael A Osborne (U Oxford)
* Collapsed Amortized Variational Inference for Switching Nonlinear Dynamical Systems
  * Zhe Dong (Google) · Bryan Seybold (Google) · Kevin Murphy (Google Brain) · Hung Bui (VinAI Research)
* Undirected Graphical Models as Approximate Posteriors
  * Arash Vahdat (NVIDIA Research) · Evgeny Andriyash (D-Wave Systems Inc.) · William Macready (D-Wave)
* On Learning Sets of Symmetric Elements
  * Haggai Maron (NVIDIA Research) · Or Litany (Stanford University) · Gal Chechik (NVIDIA / Bar-Ilan University) · Ethan Fetaya (Bar Ilan University)
* Automated Synthetic-to-Real Generalization
  * Wuyang Chen (Texas A&M University) · Zhiding Yu (NVIDIA) · Zhangyang Wang (Texas A&M University) · Anima Anandkumar (Caltech)

### Papers on Symmetric and Equivariance

* On Learning Sets of Symmetric Elements
  * Haggai Maron, Or Litany, Gal Chechik, Ethan Fetaya,
* Generalizing Convolutional Neural Networks for Equivariance to Lie Groups on Arbitrary Continuous Data
  * Marc Finzi, Samuel Stanton, Pavel Izmailov, Andrew Wilson,
* On Learning Sets of Symmetric Elements
  * Haggai Maron, Or Litany, Gal Chechik, Ethan Fetaya,
* Equivariant Flows: exact likelihood generative learning for symmetric densities.
  * Jonas Köhler, Leon Klein, Frank Noe
* Generative Flows with Matrix Exponential
  * Changyi Xiao, Ligang Liu,
* VFlow: More Expressive Generative Flows with Variational Data Augmentation
  * Jianfei Chen, Cheng Lu, Biqi Chenli, Jun Zhu, Tian Tian
* Black-Box Variational Inference as a Parametric Approximation to Langevin Dynamics
  * Matt Hoffman, Yian Ma,
