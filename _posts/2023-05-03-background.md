---
layout: post
title:  "Background"
date:   2023-05-03 20:19:47 -0500
categories: Project Abstract
---

# Tri-Plane Hybrid representation

The tri-plane serves as the foundation for the 3D GAN, enhancing its expressiveness and efficiency for 3D-aware image synthesis. The tri-plane is composed of:

- Scene features aligned on three orthogonal planes. Each feature plane has a specific resolution, such as NxNxC, where N represents the spatial resolution, and C denotes the number of channels.
- To query any 3D position x ∈ R3, project it onto each of the three feature planes, obtaining the corresponding feature vectors (Fxy, Fxz, Fyz) through bilinear interpolation and summing up the three feature vectors.
- This aggregated feature vector is then fed into a decoder (a simple MLP), which generates color and density values.
- Finally, these values are transformed into an RGB image using volume rendering techniques.
![Hybrid tri-plane representation](https://abucturab.github.io/CSCE641_Project/images/tri-plane-hybrid.png "This is an example image")