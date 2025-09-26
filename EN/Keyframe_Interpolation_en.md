# About After Effects Keyframe Interpolation

## ✍️ Overview
When working with scripts in After Effects to **copy and paste easeKeyFrame data**,  
the question came up: *which information needs to be transferred together?*  

The three key candidates are **Temporal / Spatial / Roving**.  
Since some of these terms were not entirely clear in daily use,  
I researched and整理ed their roles and meanings as a learning note.  

---

## 📌 Temporal Interpolation
- **Applies to**: All properties (Position / Opacity / Scale, etc.)  
- **Role**: Defines how values change over time  
- **Affects**: Speed Graph  

### Types & Behavior
- **Linear**  
  Icon: Diamond  
  Values change at a constant speed. Straight interpolation.  

- **Bezier**  
  Icon: Diamond (with editable handles)  
  Allows manual easing. You can freely add acceleration and deceleration.  

- **Continuous Bezier**  
  Icon: Diamond  
  Both handle angles remain aligned in a straight line.  
  Keeps smoothness while allowing influence adjustments.  

- **Auto Bezier**  
  Icon: Small circle  
  AE automatically creates the smoothest curve.  
  Once edited, it switches to Continuous.  

- **Hold**  
  Icon: Square  
  Holds the value, then jumps to the next keyframe suddenly.  

### Transition Rules
- Auto → (edit) → Continuous  
- Continuous → (move one handle) → Bezier  
- Alt + Click → Linear ⇔ Bezier  
- Hold → explicit setting only (no natural transitions)  

### Icon Guide
- Linear / Bezier / Continuous → Diamond  
- Auto Bezier → Small circle (looks similar to Roving but smaller)  
- Roving → Large circle (separate: controls timing, not interpolation)  
- Hold → Square  

### 👉 Temporal Interpolation Summary
- Linear: Straight, constant speed  
- Bezier: Manual ease, full freedom  
- Continuous: Smoothness preserved (angles aligned, lengths independent)  
- Auto: AE auto-adjust; turns into Continuous when edited  
- Hold: Value holds, then jumps  

> Temporal = "value × time change curve".  
> Spatial = "motion path shape curve".  

---

## 📌 Spatial Interpolation
- **Applies to**: Position properties only (affects motion path)  
- **Role**: Defines the trajectory an object follows  

### Types & Behavior
- **Linear**  
  Handle: None  
  Motion path is a straight line. Produces sharp corners.  

- **Bezier**  
  Handle: Independent In / Out  
  Handles can move separately. Allows free curved paths.  

- **Continuous Bezier**  
  Handle: Angles continuous / lengths independent  
  Both handle angles align in a straight line. Keeps smoothness while adjustable.  

- **Auto Bezier**  
  Handle: AE controlled  
  Both angles and lengths are auto-adjusted. Editing switches it to Continuous.  

### Transition Rules (AE behavior)
- Auto → (handle edit) → Continuous  
- Continuous → (edit one side) → Bezier  
- Alt + Click → Linear ⇔ Bezier  
- Bezier → Continuous → Right-click or scripting required  

### Internal Data
- **keyInSpatialTangent / keyOutSpatialTangent**  
  - `[dx, dy]` (or `[dx, dy, dz]` for 3D)  
  - Represents direction (angle) + length (influence) as a vector  
  - Not shown as numbers in UI; visible only as path handles  

### 👉 Spatial Interpolation Summary
- Spatial = "motion path curve"  
- Linear → Straight, no bend  
- Bezier → Fully manual curve  
- Continuous → Smooth curve with aligned angles  
- Auto → AE auto-smooth (switches to Continuous when edited)  

---

## 🟧 Roving Keyframes
- **Applies to**: Mainly Position animation (also works with other properties)  
- **Role**: AE automatically adjusts the **time positions (x-axis)** of keyframes  
- **Purpose**: Distributes timing to maintain constant or smooth speed  

### Behavior
- Normal keyframes → Fixed time positions  
- Roving keyframes → Time becomes flexible, AE redistributes positions  
- Ensures consistent overall speed across the motion  

### Icon
- Normal keyframes → Diamond (Linear/Bezier/Continuous), small circle (Auto), or square (Hold)  
- Roving keyframes → Large circle  
  - Looks similar to Auto Bezier (small circle) → difference is **size**  

### Use Case
- Perfect for motion paths where you want AE to balance speed automatically  
- Example: A car follows a road → Start and end are fixed, intermediate keys are “smoothed” for even speed  

---

## 📝 Overall Summary
- **Temporal** → Defines how speed changes over time  
- **Spatial** → Defines the path shape (straight/curved)  
- **Roving** → Lets AE balance the timing (x-axis positions)  

👉 Roving is a special mode: it changes the **time coordinates themselves**,  
making it a different kind of control than standard interpolation.  
