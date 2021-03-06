---
layout: page
permalink: /starmap/specifications/properties/
status:
  date: February 19, 2017
  is: Stable
interfacelevel: L2
implementationlevel: L4
library: material-motion
---

# Connecting streams

This is the engineering specification for connecting streams with existing information.

## Overview

Values are the data emitted by a MotionObservable. Values are generated by **systems** and are
eventually written to **reactive properties**. In practice, the most common types of values are:

- Floating point numbers.
- Multi-dimensional positions.
- Rectangles.
- Sizes.

<iframe width="100%" height="220" frameborder="0" src="https://material-motion.github.io/loopy/v1/?embed=1&data=[[[1,399,304,1,%22System%22,5],[2,714,310,1,%22Reactive%2520Property%22,4]],[[1,2,89,1,0]],[[558,214,%22Values%22]],2%5D"></iframe>

### Systems generate values

A **system** emits values on an observable's `next` channel.

Some examples:

- Scroll events from a scroll view.
- Values from a physics simulation.
- Interpolated values.
- Gesture recognition events.

Systems are often **reactively configurable**, meaning changes to their configuration are expected
to be immediately reflected in the values they emit. For example, a Spring's *tension* can be
changed, causing the spring's simulation to immediately be affected.

### Reactive properties store values

A **reactive property** is an object that stores a value of generic type T. Reactive properties
must be initialized with a default value. Reactive properties can be observed like a
MotionObservable.

Some examples:

- A view's position.
- A transition's direction.

#### A note on properties representing existing information

Reactive properties that represent existing information are often configured to update the existing
information when the property changes. This is sometimes referred to as "external writes" within
the property implementations. Consider the following diagram, in which writes made to the reactive
property are propagated to the external information:

<iframe width="100%" height="220" frameborder="0" src="https://material-motion.github.io/loopy/v1/?embed=1&data=[[[1,513,461,1,%22System%22,5],[2,759,461,1,%22Reactive%2520Property%22,4],[4,983,462,1,%22Existing%2520information%22,1]],[[1,2,3,1,0],[2,4,3,1,0]],[],4%5D"></iframe>

Reactive properties that represent existing information don't know when the existing information
has changed. It is the responsibility of the existing information's owner to keep the property
synchronized. Consider the following code example:

```swift
// Some existing information
element.opacity = 0.5

// Create a property representing the external information
let opacity = runtime.get(element).opacity

opacity.value // 0.5

// Changing the property will cause an external write to the existing information.
opacity.value = 1.0
opacity.value   // 1.0
element.opacity // 1.0

// Changing the existing information will not update the property.
element.opacity = 0.0
opacity.value   // 1.0
element.opacity // 0.0
```
