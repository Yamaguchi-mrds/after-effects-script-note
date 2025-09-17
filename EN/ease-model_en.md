# After Effects Keyframe Easing Mathematical Model

## 1. Basic Concept: Speed and Influence

In Adobe After Effects, the **speed graph** represents the rate of change (velocity) of a property’s value over time.
Each keyframe has the following two parameters:

* **speed**

  * The slope of the graph (units: value/sec)
  * Determines how steeply the value changes
* **influence**

  * The length of the handle in the time direction (percentage)
  * Higher values extend acceleration/deceleration over a longer portion of the segment

**Example:**
The default “Easy Ease” sets incoming/outgoing speeds to 0 and influence to 33.33%, producing an S-curve shape very close to a sine wave.

---

## 2. How Speed and Influence Affect Bézier Handles

```
Time →
  [Key1]●───╮
             ╰─────●[Key2]

     ↑ Horizontal distance = segment duration × (influence / 100)
     ↑ Vertical distance   = speed × horizontal distance
```

* **Horizontal distance (time axis)** = segment length × (influence ÷ 100)
* **Vertical distance (value axis)** = speed × horizontal distance

These two values determine the handle angle, which defines the shape of the ease curve.

---

## 3. Mathematical Model (Hermite Interpolation)

Using a normalized parameter **u** from 0 to 1 for the interval:

```jsx
p(u) = (2u^3 - 3u^2 + 1) * p0 
     + (u^3 - 2u^2 + u)   * m0
     + (-2u^3 + 3u^2)     * p1
     + (u^3 - u^2)        * m1
```

* p0, p1 = keyframe values
* m0, m1 = tangents
  m0 = speed\_out × segment duration
  m1 = speed\_in  × segment duration

This formula produces a smooth S-curve from the given speed and influence.

---

## 4. ExtendScript Interpolation Function Example (Safe Version)

```jsx
function interpolateEase(val1, val2, t1, t2, vOut1, vIn2) {
    var dt = t2 - t1;
    var u = (time - t1) / dt; 
    var m0 = vOut1 * dt;
    var m1 = vIn2 * dt;
    var h00 =  2*u*u*u - 3*u*u + 1;
    var h10 =      u*u*u - 2*u*u + u;
    var h01 = -2*u*u*u + 3*u*u;
    var h11 =      u*u*u -   u*u;
    return h00*val1 + h10*m0 + h01*val2 + h11*m1;
}
```

---

## 5. Summary Diagram

```
Value ↑
     |
p1   |                     ●[Key2]
     |                  ／
     |               ／
     |            ／
p0   |●[Key1]───╯
     +--------------------------------→ Time axis
          ← influence → 
     speed: vertical rise
     influence: horizontal stretch
```

---

## References

* [Adobe After Effects Scripting Guide – KeyframeEase](https://ae-scripting.docsforadobe.dev/other/keyframeease.html)
* [Adobe Help – Keyframe Interpolation](https://helpx.adobe.com/after-effects/using/keyframe-interpolation.html)