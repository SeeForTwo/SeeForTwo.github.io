---
layout: default
title: "SeeForTwo: Detecting Left and Right Eyes"
---

## Computer Vision: Detecting Left and Right Eyes

This experiment compares two similar but not identical object detection
network structures (Convolutional Neural Network, CNN; Deep Neural
Network, DNN). Both are capable of the task of
detecting human eyes (i.e. one class). Only one of these two network
structures has relatively good performance for detecting left or right
human eyes (i.e. two classes of similar objects). The motivation for
this experiment is an interest in understanding the minimal requirements
for network structures for particular object detection applications.

![Detection input images: all good detections](/assets/img/eyes_005_all_detected.jpg)
![Detection results for eyes](/assets/img/both_eyes_005_recog.jpg)
![Detection results for left or right eyes](/assets/img/lr_eyes_005_recog.jpg)

Software from the 
[TensorFlow Object Detection API](https://github.com/tensorflow/models/tree/master/research/object_detection)<sup>[1]</sup> and images cropped from
[Open Images Dataset](https://storage.googleapis.com/openimages/web/index.html)<sup>[2,3]</sup> were used.
A library for
[putting annotations in a hierarchy](hierarchical_annotations)
was used to automate labeling eyes as right eyes or left eyes<sup>*</sup>.
Above are some example images where both network structures gave
correct detections for both tasks.
![F1-scores for two object detectors for two tasks](/assets/img/f1_005.jpg)
F1-scores for the two network structures, "A" and "B" are plotted
above.  The top of the blue rectangle is a network structure's
performance on the left or right eye detection task.  The top of the
gray hashed rectangle is that network structure's performance on the
eye detection task.  (So in general, "A" is a better recognizer than
"B" for eyes.) This top serves as an upper bound on the performance on
the left or right eye detection task.  The bottom of the blue
rectangle is what the F1-score would be for a "bad left/right eye detector"
that detected
eyes and just randomly guessed left eye or right eye. This bottom
serves as a lower bound on the performance of detection for the left
eye or right eye task. Network structure "A" has performance on the
left eye or right eye task that is closer to its upper bound than its
lower bound which is relatively good performance. In contrast,
structure "B" has performance on the left eye or right eye task that
is closer to its lower bound than its upper bound which is relatively
bad performance.

![Detection input images: only B left or right eye errors](/assets/img/eyes_005_only_b_lr_error.jpg)

Above are some example images where both network structures gave
correct detections when trained for the "eyes" task, but only
network structure "A" gave gave correct detections when trained for
the "left or right eyes" task.

![Detection input images: all errors](/assets/img/eyes_005_all_error.jpg)
![Detection input images: negative images](/assets/img/eyes_005_negatives.jpg)

Finally, to show the diversity of the images, above are two rows of
examples. The first row has images where both network structures gave
incorrect detections for both tasks. The second row has negative
example images (images that do not contain eyes) where both network
structures correctly did not have detections for both tasks.

## Bibliography

[1] "Speed/accuracy trade-offs for modern convolutional object detectors."
Huang J, Rathod V, Sun C, Zhu M, Korattikara A, Fathi A, Fischer I, Wojna Z,
Song Y, Guadarrama S, Murphy K, CVPR 2017

[2] A. Kuznetsova, H. Rom, N. Alldrin, J. Uijlings, I. Krasin, J. Pont-Tuset, S. Kamali, S. Popov, M. Malloci, A. Kolesnikov, T. Duerig, and V. Ferrari.
The Open Images Dataset V4: Unified image classification, object detection, and visual relationship detection at scale.
IJCV, 2020.

[3] Krasin I., Duerig T., Alldrin N., Ferrari V., Abu-El-Haija S., Kuznetsova A., Rom H., Uijlings J., Popov S., Kamali S., Malloci M., Pont-Tuset J., Veit A., Belongie S., Gomes V., Gupta A., Sun C., Chechik G., Cai D., Feng Z., Narayanan D., Murphy K.
OpenImages: A public dataset for large-scale multi-label and multi-class image classification, 2017.

## Notes

&#42; Eyes in images were labeled "left" or "right" based on the side of
the face in the image, which matches what a person would do if looking
at oneself in a mirror. The opposite labeling would not change overall
performance.

[Home](./)
