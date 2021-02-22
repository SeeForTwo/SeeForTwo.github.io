---
layout: default
title: "SeeForTwo: Computer Vision"
og_rel_url: ""
og_rel_image: assets/img/SeeForTwo_eyes_640x320_margin.jpg
og_description: Detecting left and right eyes is an experiment in an area of personal interest, understanding the minimal requirements for network (Convolutional Neural Network, CNN / Deep Neural Network, DNN) structures for particular object detection (computer vision) applications.
---

## {{ site.description }}

[Detecting left and right eyes](left_right_eyes) describes an
experiment in an area of personal interest, understanding the minimal
requirements for network (Convolutional Neural Network, CNN / Deep
Neural Network, DNN) structures for particular object detection
(computer vision) applications.  A spin-out of this work is
<https://github.com/SeeForTwo/hier_object>, a Python library for
[putting annotations in a hierarchy](hierarchical_annotations) that
groups ground truth object bounding boxes.  The library is well tested
using property based testing, [random testing for hierarchical
annotations](random_testing).

![Detection input images](/assets/img/eyes_005_all_detected.jpg)
![Detection results for left or right eyes](/assets/img/lr_eyes_005_recog.jpg)

## Why "SeeForTwo"?

The title "SeeForTwo" comes for the informal standard of judging computer
vision performance using a comparison against a human that only has
two seconds to look at the image. For example,
<https://cloud.google.com/vision/automl/docs/prepare> says:
   
> AutoML Vision models can't generally predict labels that humans
> can't assign. So, if a human can't be trained to assign labels by
> looking at the image for 1-2 seconds, the model likely can't be
> trained to do it either.

---

I can be contacted via a gmail account that matches my github account.
Any other use of "SeeForTwo" on the internet is not related.
