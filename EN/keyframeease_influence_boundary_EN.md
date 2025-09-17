# 📚 The KeyframeEase 0.1 Issue and Its Workarounds

### Official Specification

- According to the After Effects Scripting Guide, the **influence argument of KeyframeEase is defined in the range 0.1–100**.  
- However, the boundary value of `0.1` is not always accepted reliably.  

---

### Reported Behavior in Practice

- When calling `KeyframeEase(..., 0.1)` via script:  
  - Sometimes it works,  
  - and sometimes it throws an error: **“parameter 2 out of range 0.1 to 100”**.  
- Even though the value looks like exactly `0.1`, the result may differ depending on internal processing or environment.  

---

### Common Causes

- **Floating-point precision errors**  
  - Depending on the calculation path, `0.1` may actually become `0.099999…`, resulting in an out-of-range error.  

- **Differences in AE internal state**  
  - The handling may differ depending on the source of the value (calculated result, constant, or UI input), causing inconsistent acceptance.  

- **Strictness of the API check**  
  - Values accepted via the UI may still be rejected by the scripting API due to stricter validation.  

---

### Consistency with External Reports

- Adobe forums and user reports confirm that:  
  - **“Small values can be entered via UI, but passing the same values through a script throws an error.”**  
- This indicates that AE’s internal floating-point handling and boundary checks are not always aligned with the UI behavior.  

---

### Practical Guidelines

- **Do not rely on boundary values (exactly 0.1 or exactly 100).**  
- When handling influence values in scripts:  
  - Use **0.10001 or higher** for the lower bound  
  - Cap at **100** for the upper bound  
  - Round values before passing them to AE to avoid errors  

---

### Example Utility Function
```jsx
// Utility function to ensure influence values stay within AE's expected range (0.1–100).
// Especially for the lower bound, 0.1 is unstable, so round up to 0.10000001 for safety.
function safeInfluence(x) {
    if (x < 0.10001) return 0.10000001; // Lower bound
    if (x > 100) return 100;           // Upper bound
    return x;
}
```

### ✅ Summary

- 0.1 is allowed by spec, but may cause errors depending on AE’s internal state.
- Stabilize by rounding up using safeInfluence() (ensure ≥ 0.10001).
- Be cautious of AE API’s handling of boundary values and internal rounding behavior.