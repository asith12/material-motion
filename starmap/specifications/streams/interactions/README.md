---
layout: page
permalink: /starmap/specifications/streams/plans/
title: Interactions
status:
  date: December 15, 2016
  is: Draft
knowledgelevel: L2
library: streams
---

# Interactions

An interactions is an object that creates streams from a set of objects and properties.

For example:

```swift
class TossableInteraction {
  let spring = Spring<CGPoint>
  let positionStream = MotionObservable<CGPoint>
  let initialVelocityStream = MotionObservable<CGPoint>

  init(destination: Property<CGPoint>, view: UIView) {
    let dragGesture = DragGestureRecognizer()
    let dragStream = gestureSource(dragGesture)
    initialVelocityStream = dragStream.onRecognitionState(.ended).velocity(in: view)

    let spring = Spring(to: destination,
                        initialValue: propertyOf(view).center,
                        initialVelocity: property(withInitialValue: CGPoint(x: 0, y: 0)))
    let springStream = springSource(spring)
    positionStream = springStream.toggled(with: dragStream.centroid(in: view))
  }
}

let tossable = TossableInteraction(destination: someDestination, view: view)

aggregator.write(tossable.initialVelocityStream,
                 to: tossable.spring.initialVelocity)
aggregator.write(tossable.positionStream,
                 to: propertyOf(view).center)
```