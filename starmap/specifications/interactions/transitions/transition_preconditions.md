---
layout: page
statuses:
  - status:
    date: Oct 25, 2016
    type: Drafting
---


# Transition preconditions feature specification

## MVP

A transition can register certain pre-conditions. If any pre-condition fails, the director will not be selected for use in the transition.

**Preconditions API**: Provide an API for returning preconditions.

Example pseudo-code:

```
TransitionDirector {
  preconditions() -> [Precondition] {
    return [Precondition({
      return elementExists
    })]
  }
}
```

