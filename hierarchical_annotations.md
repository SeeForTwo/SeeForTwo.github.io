---
layout: default
title: "SeeForTwo: Putting Annotations in a Hierarchy"
---

## Computer Vision: Putting Annotations in a Hierarchy

<https://github.com/SeeForTwo/hier_object> contains a Python library
for putting computer vision ground truth object bounding box
annotations into a hierarchy.  It groups smaller objects based on
larger objects that contain them.  For example, given independent
annotations for human faces and human eyes, it can determine which
eyes are in each face.  (Knowing with eyes are in each face is a good
first step for creating ground truth data that distinguishes left
eyes and right eyes.)  The library's goal is to automatically handle all
the usual cases and to be used to identify a hopefully small number of
remaining unusual cases so those can be handled manually.

![Overlapping faces](/assets/img/overlapping_faces_ab8c8a.jpg)

Above is an example (using an image and annotations from the
[Open Images Dataset](https://storage.googleapis.com/openimages/web/index.html)<sup>[1,2]</sup>)
of an unusual case where head and face bounding boxes have significant overlap.
The bounding boxes for two of the four eyes in this example are inside
both head bounding boxes and overlap (and/or are inside) both face bounding
boxes.

The library handles the following:

* The hierarchy levels used in the annotations do not have to be
  consistent. The library can group the following based on the most
  appropriate containing object, for example:
   * eye annotations inside face annotations,
   * eye annotations inside head annotations,
   * eye annotations inside face annotations inside head annotations.

* Containing objects can overlap. For example, heads that have large bounding
  boxes due to hair that extend far from the corresponding face.
  
* Objects can have duplicate annotations (i.e. similar bounding boxes,
  possibly but not necessarily identical), where perhaps the same
  object was annotated twice.
 
* The bounding boxes of objects contained by another object can extend
  outside the object that contains it. This handles independent
  human annotations where a tighter bounding box was provided for
  the larger object.

An experiment in
[detecting left and right eyes](left_right_eyes)
used this library. The library is well tested using property based testing, see [Random Testing for Hierarchical Annotations](random_testing).

---

P.S. If the goal instead was a best effort fully automated system, then optimization
software such as
[Google OR-Tools](https://developers.google.com/optimization/assignment/overview)
could used to find assignments in ambiguous cases
such as the overlapping faces in the example above. 

## Bibliography

[1] A. Kuznetsova, H. Rom, N. Alldrin, J. Uijlings, I. Krasin, J. Pont-Tuset, S. Kamali, S. Popov, M. Malloci, A. Kolesnikov, T. Duerig, and V. Ferrari.
The Open Images Dataset V4: Unified image classification, object detection, and visual relationship detection at scale.
IJCV, 2020.

[2] Krasin I., Duerig T., Alldrin N., Ferrari V., Abu-El-Haija S., Kuznetsova A., Rom H., Uijlings J., Popov S., Kamali S., Malloci M., Pont-Tuset J., Veit A., Belongie S., Gomes V., Gupta A., Sun C., Chechik G., Cai D., Feng Z., Narayanan D., Murphy K.
OpenImages: A public dataset for large-scale multi-label and multi-class image classification, 2017.

[Home](./)
