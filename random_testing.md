---
layout: default
title: "SeeForTwo: Random Testing for Hierarchical Annotations"
og_rel_url: random_testing
og_rel_image: assets/img/SeeForTwo_test_640x320.jpg
og_description: Testing hier_object, a Python library for putting annotations in a hierarchy, using the Hypothesis Library for property based (random) testing.
---

## Computer Vision: Random Testing for Hierarchical Annotations

Object bounding boxes used as test data for `hier_object`, a Python library for
[putting annotations in a hierarchy](hierarchical_annotations) (with software available in
<https://github.com/SeeForTwo/hier_object>),
needed to include the following cases:

* The hierarchy levels used in the annotations do not have to be
  consistent.
* Containing objects can overlap.
* Objects can have duplicate annotations.
* The bounding boxes of objects contained by another object can extend
  outside the the object that contains it.

![examples of test data](/assets/img/random_objects.jpg)

Instead of hand selecting examples from ground truth data or hand
creating examples, the Hypothesis library for property based testing
(inspired by Haskell's QuickCheck) was used to create pseudo-random examples.
See
<https://hypothesis.works/> and <https://hypothesis.readthedocs.io/en/latest/>.
Four examples with varying complexity are plotted on gray background images above.
The documentation for the Hypothesis library summarizes what it does as:

> It works by generating arbitrary data matching your specification
> and checking that your guarantee still holds in that case. If it
> finds an example where it doesn’t, it takes that example and cuts it
> down to size, simplifying it until it finds a much smaller example
> that still causes the problem. It then saves that example for later,
> so that once it has found a problem with your code it will not
> forget it in the future.

## An example

Below is the (correct) `is_inside` function in `hier_object` (the
library to be tested) that determines if a first object (`self`)
is inside a second object (`other`).
It compares corresponding bounding box coordinate variables:
`x0` with `x0`, `x1` with `x1`, `y0` with `y0` and *`y1` with `y1`*.

```python
def is_inside(self, other):
    """
    Return True if this object is inside (or the same as) OTHER.
    """
    return ((self.x0 >= other.x0)
            and (self.x1 <= other.x1)
            and (self.y0 >= other.y0)
            and (self.y1 <= other.y1))
```

To illustrate what the Hypothesis library does, below is the
`is_inside` function modified with an artificial bug. When the first
object (`self`) is in the lower right quarter of the image (i.e. X and Y
coordinates greater than 0.5), an incorrect comparison between `y1` and
`x1` is used. So this artificial bug only manifests for test data
in this quarter of the image when the X and Y coordinates of the lower
right corner of the second (`other`) object are different.

```python
def is_inside(self, other):
    """
    Return True if this object is inside (or the same as) OTHER.
    """
    if (self.x0 > 0.5) and (self.y0 > 0.5): # Condition for artificial bug, lower right corner
        return ((self.x0 >= other.x0)
            and (self.x1 <= other.x1)
            and (self.y0 >= other.y0)
            and (self.y1 <= other.x1)) # Artificial bug, comparing y1 with x1, not y1 with y1
    return ((self.x0 >= other.x0)
            and (self.x1 <= other.x1)
            and (self.y0 >= other.y0)
            and (self.y1 <= other.y1))
```

![Example of ???](/assets/img/is_inside_demo1d.jpg)

The image above is a plot of one of four examples found by the Hypothesis library.
These examples cause four different asserts in `test_hyp_hier_object.py` to fail:
* missing inside object when there is another inside object of the same class,
* missing inside object when there is no other inside object of the
  same class but inside objects of another class,
* missing inside object when there are other no inside objects of any class, and
* missing outer object.

The test software for `hier_object`, allocates space for groups of
objects that can overlap and then generates objects in the portion of
the space for its group.  This guarantees that objects in different
groups can never be inside one another or duplicates.  For example,
the test software has an `object_is_inside` function (in
`bbox_utils.py`) that is equivalent to `is_inside`. Since
`object_is_inside` is never used with pairs of objects in different
groups, if it had an equivalent artificial bug as the one in
`is_inside`, this case would still be detectable (since `is_inside` is
used on objects in different groups). In fact, the Hypothesis library
did find two examples that caused two different asserts to fail when
both artificial bugs were present.

## Conclusion and future thoughts

For more detail about the test software, see the [github repo
README](https://github.com/SeeForTwo/hier_object/blob/main/test/README.md).

Passing the Hypothesis based tests shows that two pieces of software
followed exactly the same rules, one (the `hier_object` library) that
was written as the cases that it needed to be handled were discovered
and the other (the test software) that was written with those cases
in mind. This gave confidence that `hier_object` library was correct
before starting debugging and tuning of the software that used it
(e.g. it was correct before tuning the threshold for duplicates.).

One alternative to how this test software operates would be to use
`hypothesis.strategies.recursive` to generate examples.  This would
simplify the software. Simple recursion would work for arbitrary
floating point bounding boxes but it is not clear how to limit
bounding boxes from getting smaller than what could correspond to
integer pixel sizes in an image.

Other alternatives would be to add more constraints to objects
generated. This could be done to match needs for testing particular
business logic that is applied to recognition results. Or this could be
done so that actual images could be created by using the examples to
drive image processing, computer graphics rendering (e.g. with Blender
software) and/or with Generative Adversarial Networks (GAN).

Finally, in addition to being able to test libraries like `hier_object`,
the Hypothesis library can test stateful things. So it could be used
to test stateful web service logic.

[Home](./)



