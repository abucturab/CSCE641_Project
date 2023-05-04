---
layout: post
title:  "Experiment"
date:   2023-05-01 20:19:47 -0500
categories: Project Abstract
order: 3
---

In Order to Test the effectiveness of the Tri-Plane for Encoding the 3d features and Geometry, I have tested it on a Single Scene. This is an effective way to compare the performance of the Tri-Plane and a significantly smaller MLP used in the EG3D paper with that used in the original NeRF implementation.

# Dataset

The dataset used is from the [ShapeNet](https://shapenet.org/) dataset, which consists of richly-annotated, large-scale dataset of 3D shapes. The data was downloaded from the dataset used in the paper - [“Scene Representation Networks: Continuous 3D-Structure-Aware Neural Scene Representations”](https://www.vincentsitzmann.com/srns/) and I am using the [“cars_train”](https://drive.google.com/file/d/1bThUNtIHx4xEQyffVBSf82ABDDh2HlFn/view?usp=share_link) file from their hosted data, which consists of images of cars from multiple angles.

The GitHub repository provided by EG3D developers have a script for pre-processing the dataset so that it is compatible for training

<pre>
cd dataset_preprocessing/shapenet 
python run_me.py 
</pre>

For the Baseline NeRF, there is a script "dataloader.py" which does the preprocessing and sets up the dataset directory. The README in the [baseline NeRF](https://github.com/abucturab/NeRF_641_project) repo has further details

```python dataloader.py /path/to/object/dataset```

# Training

In order to check the effectiveness of the Tri-Plane and the Neural Rendering used in EG3D, we need a trained Tri-Plane which has encoding for the 3D structure and geometry of a collection of objects. The collection of objects or a category could be Human faces, Animal faces, collection of similar objects, etc., In our experiment we are using a collection of Cars. The Tri-plane is trained as -

1. Setup the training data -
    - the dataset consists of **2150 cars** with around **250 images of each car** from multiple angles
    - remove one of the car from the dataset, which is to be used to test the Tri-plane and the neural rendering quality for a single scene

2. Training the 3D GAN pipeline end-to-end -
    - In order to get a learned feature map which can be used as the planes of the Tri-plane, we train the 3D GAN end to end on the cars dataset
    - This results in a trained Generator, which is able to produce a feature map which has the 3d geometry information encoded in it
    - We train the network for **2500K epochs**, which takes around **3-4 days** on **2 Nvidia A100 GPU**

the newly trained model doesn’t have the same fidelity as the paper mentions since it is trained 10x less than what the original paper trains it for since they use a **8 GPU** setup. However it still has a good generation FID comparable to other papers as - [Pi GAN](https://marcoamonteiro.github.io/pi-GAN-website/) or [GIRAFFE](https://m-niemeyer.github.io/project-pages/giraffe/index.html) **(29-32)**

![Trained GAN generated images](https://abucturab.github.io/CSCE641_Project/images/trained-gan.png "Trained GAN generated images")

Now that we have a trained generator which can produce a feature map we use the dataset ,i.e., the car images which were removed from the initial dataset to train the decoder for single scene overfitting. we train as follows -

1. Freeze the Discriminator 
2. Freeze the Generator
3. Train the single scene for **200K-100K Epochs**, which takes **1-2 days** on **2 Nvidia A100 GPUs**

Since the Generator is frozen, we have more or less a static feature plane which has learned some 3d geometry and other features common and prevalent in the cars dataset. The decoder (Ray marching, MLP and Volume rendering pipeline) learns the weights of the MLP.

# Baseline NeRF

In order to compare the quality and the performance of the Tri-plane representation for Neural Rendering and the power of the Tri-plane to capture the 3d geometry, we compare it with the original NeRF implementation.

We use the same dataset, i.e., car which was used to train the decoder in the 2nd part of the above experiment. 

The settings of the NeRF are as follows -

- Using **image resolution of 64X64**
- Using **200 Images** from different poses for training and **50 for Validation**
- Training the NeRF for **100K iteration**, although couldn't run more than 2000 epochs since the training is very slow
- Hierarchical sampling with **64 coarse samples** per ray and **128 Fine samples** per ray.

Due to limitations in hardware and issues in not being able to get A100 GPUs, the Baseline NeRF is trained using **1 V100 GPU**. It takes around **2-3 days** for **2K epochs**

