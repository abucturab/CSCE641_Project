---
layout: post
title:  "Background"
date:   2023-05-02 20:19:47 -0500
categories: Project Abstract
order: 2
---

# Tri-Plane Hybrid representation

The tri-plane serves as the foundation for the 3D GAN, enhancing its expressiveness and efficiency for 3D-aware image synthesis. The tri-plane is composed of:

- Scene features aligned on three orthogonal planes. Each feature plane has a specific resolution, such as NxNxC, where N represents the spatial resolution, and C denotes the number of channels.
- To query any 3D position x ∈ R3, project it onto each of the three feature planes, obtaining the corresponding feature vectors (Fxy, Fxz, Fyz) through bilinear interpolation and summing up the three feature vectors.
- This aggregated feature vector is then fed into a decoder (a simple MLP), which generates color and density values.
- Finally, these values are transformed into an RGB image using volume rendering techniques.
![Hybrid tri-plane representation](https://abucturab.github.io/CSCE641_Project/images/tri-plane-hybrid.png "Hybrid tri-plane representation")

# 3D GAN using tri-plane

The Image below summarized the entire pipeline for the 3D scene generation -

![EG3D GAN e2e pipeline](https://abucturab.github.io/CSCE641_Project/images/eg3d-gan-e2e.png "EG3D GAN e2e pipeline")

As observed, the input to the neural renderer consists of sample points from the tri-plane domain. The generator, which is based on the StyleGAN2 architecture, produces feature maps using its convolutional layers. These feature maps are distributed among three orthogonal, axis-aligned planes. Similar to NeRF, rays are projected from the camera origin to each pixel on the image plane, and ray marching is performed on each ray.

The sampling points are projected onto the tri-planes to obtain their values, which are then passed through the MLP to determine their color and density values. Since the projected sample points already encode some 3D structure, the MLP's size is much smaller compared to NeRF, utilizing only two layers of fully connected networks. This design is expected to significantly enhance the performance of neural rendering and image generation.

