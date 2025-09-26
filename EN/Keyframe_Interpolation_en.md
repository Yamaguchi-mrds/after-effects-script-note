# About After Effects Keyframe Interpolation

## ✍️ Overview

When copying and pasting **easeKeyFrame data** via script in After Effects,
the question arises of which information should be transferred along with it.
Three candidates came up: **Temporal / Spatial / Roving**.

This document summarizes the meaning of each interpolation type and flag as my personal study notes.

---

📑 Table of Contents

1. Temporal Interpolation
   Determines how the value changes over time. Directly tied to the Speed Graph.

2. Spatial Interpolation
   Determines which path the object follows. Affects the Motion Path.

3. Roving
   Lets AE automatically adjust the **time positions** of keyframes to even out overall speed.

---

## 1. Temporal Interpolation 📌

* **Target**: All properties (Position / Opacity / Scale, etc.)
* **Role**: Defines how values change over time
* **Affects**: Speed Graph

### Types and Behavior

* **Linear**
  Icon: Diamond
  Changes at a constant rate. Simple linear interpolation.

* **Bezier**
  Icon: Diamond (with adjustable handles)
  Allows manual easing. Full control over speed variations.

* **Continuous Bezier**
  Icon: Diamond
  In/out handle angles stay aligned. Keeps smoothness while adjusting influence.

* **Auto Bezier**
  Icon: Small circle
  AE automatically sets the smoothest curve. Editing converts it to Continuous.

* **Hold**
  Icon: Square
  Holds the value until just before the next keyframe, then jumps.

### Transition Rules

* Auto → (adjust handle) → Continuous
* Continuous → (adjust one side handle) → Bezier
* Alt+Click → Linear ⇔ Bezier
* Hold must be explicitly set (no natural transition)

### Icon Distinction

* Linear / Bezier / Continuous → Diamond
* Auto Bezier → Small circle (looks like Roving but smaller)
* Roving → Large circle (separate: time-position adjustment)
* Hold → Square

```jsx
// ScriptCode

// Get
var tInterp = myProp.keyInInterpolationType(k);  
// → KeyframeInterpolationType.LINEAR / BEZIER / HOLD

// Set
myProp.setInterpolationTypeAtKey(k, KeyframeInterpolationType.BEZIER);

```

### 👉 Temporal Summary

* Linear: straight line, constant speed
* Bezier: free easing
* Continuous: smooth constraint easing (angles aligned, length free)
* Auto: AE auto-curve, turns into Continuous when touched
* Hold: value hold → sudden jump

> Temporal = "value × time curve",
> Spatial = "path shape curve". They work as a pair.

---

## 2. Spatial Interpolation 📌

* **Target**: Position properties only (affects Motion Path)
* **Role**: Defines the path the object follows

### Types and Behavior

* **Linear**
  No handles
  Path is straight segments, producing angular motion.

* **Bezier**
  Handles: In / Out independent
  Each handle can be adjusted separately for free bending.

* **Continuous Bezier**
  Handles: Angle continuous / Length independent
  Handle angles always stay in a straight line. Smooth curve while adjusting.

* **Auto Bezier**
  Handles: AE auto-controls
  Angle and length both optimized automatically. Editing turns it into Continuous.

### Transition Rules (AE’s logic)

* Auto → (handle adjustment) → Continuous
* Continuous → (one handle adjusted) → Bezier
* Alt+Click → Linear ⇔ Bezier
* Bezier → Continuous requires right-click or scripting

### Internal Data

* keyInSpatialTangent / keyOutSpatialTangent

  * `[dx, dy]` (or `[dx, dy, dz]` in 3D) vectors
  * Encodes direction (angle) and influence (length)
  * No direct numeric UI values, only visible via handle display

```jsx
// ScriptCode

// Get
var inTan  = myProp.keyInSpatialTangent(k);   // [dx, dy]
var outTan = myProp.keyOutSpatialTangent(k);  // [dx, dy]

// Set
myProp.setSpatialTangentsAtKey(k, [10, 0], [-10, 0]); 
myProp.setSpatialContinuousAtKey(k, true);    // Continuous flag
myProp.setSpatialAutoBezierAtKey(k, true);    // Auto flag

```

### 👉 Spatial Summary

* Spatial = “path curve”
* Linear → sharp corners
* Bezier → full manual control
* Continuous → smooth constrained control
* Auto → AE auto-curve, turns into Continuous when touched

---

## 3. Roving 🟧

* **Target**: Mainly Position animation (but can be used for other properties)
* **Role**: AE automatically adjusts time-axis positions of keyframes
* **Purpose**: Keeps overall speed uniform, or redistributes smoothly

### Behavior

* Normal keyframes = fixed time position
* Roving = time position unlocked, becomes fluid
* AE repositions keyframes to maintain even speed as much as possible

### Icons

* Normal keys → Diamond (Linear/Bezier/Continuous), Small circle (Auto), Square (Hold)
* Roving keys → Large circle

  * Looks like Auto Bezier, but larger
  * Key difference = size of the circle

### Use Cases

* Position animation motion paths where
  “intermediate keyframes should be evenly distributed by AE”
* Example: A car driving on a road → Start and end fixed, intermediate points auto-distributed evenly

```jsx
// ScriptCode

// Get
var isRoving = myProp.keyRoving(k);  // true / false

// Set
myProp.setRovingAtKey(k, true);

```

---

## 📝 Summary

* **Temporal** → Defines how speed changes
* **Spatial** → Defines which path is followed
* **Roving** → Lets AE adjust time positions to even out motion

👉 Roving is a **special mode that makes the time-axis coordinate itself variable**.
It is fundamentally different from regular keyframes.

---
