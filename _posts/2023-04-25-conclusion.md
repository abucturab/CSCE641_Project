---
layout: post
title:  "Conclusion"
date:   2023-04-25 18:19:47 -0500
categories: Project Conclusion
order: 5
---

The Tri-Plane representation offers a significant performance boost compared to the original NeRF implementation. The primary advantage of the Tri-plane lies in its ability to learn a feature map that encodes 3D geometry, thereby reducing the burden on the MLP. This reduction in the size of the neural network used for volume rendering leads to a substantial acceleration of the process.

Despite the considerable improvement in performance, acquiring a learned feature map for the Tri-Plane remains an arduous task. In future research, we aim to explore more straightforward and generalized methods for obtaining these feature maps. The current approach relies on the assumption that all images in the dataset belong to a similar class and possess some geometric similarities that can be encoded. It would be intriguing to investigate how a feature map trained on a different but related dataset would perform in generating a 3D representation of a target object. For instance, if a feature map encoding trains were used to create a 3D representation of cars, what would the results look like?