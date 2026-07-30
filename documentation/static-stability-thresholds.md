# Static-Stability Threshold Research and Validation

## 1. Purpose

This document establishes the engineering interpretation of the initial static-margin ranges used by version 0.1 of the **Rocket Design Diagnostic Dashboard**.

The conclusions in this document are intended to support the MVP's rule-based diagnostic logic. They define which static-margin ranges satisfy the MVP criterion, which ranges require additional review, and which ranges do not demonstrate positive initial static stability.

This document does **not**:

- Define the final user-facing diagnostic-message catalog.
- Certify that a rocket is safe, legal, or ready to fly.
- Replace experienced engineering review, physical inspection, testing, manufacturer instructions, or launch-organization requirements.
- Establish a complete dynamic-stability evaluation.

---

## 2. Scope of the MVP Interpretation

The threshold classification defined here applies to the **initial static margin of the fully configured rocket at motor ignition**.

For the MVP, this means the static margin associated with:

- The rocket's launch-ready configuration.
- The selected motor installed and loaded with propellant.
- The user-provided center of gravity.
- The calculated center of pressure at the initial reference condition used by RocketPy.
- Time equal to zero at motor ignition.

RocketPy distinguishes between static margin and flight stability margin. Its flight stability margin can account for changes in center of pressure with Mach number and changes in center of gravity as propellant mass evolves [3][4]. Therefore, the initial static-margin classification must not be presented as proof that the same margin remains valid throughout the complete flight.

The MVP may display time-varying stability information where the validated RocketPy implementation supports it, but the four threshold ranges in this document classify the **initial** result only.

---

## 3. Engineering Definitions

### 3.1 Center of Gravity

The center of gravity, abbreviated **CG**, is the point about which the rocket's mass is balanced and about which the rocket rotates. Its location depends on the mass and position of the rocket's components, including the installed motor and propellant [1].

### 3.2 Center of Pressure

The center of pressure, abbreviated **CP**, is the effective point through which the resultant aerodynamic force acts. Its calculated location depends on the rocket geometry, aerodynamic model, Mach number, and other assumptions used by the simulation [1][3].

### 3.3 Static Stability

A conventionally fin-stabilized rocket has positive static stability when the CP is located aft of the CG. When the rocket is disturbed through a small angle of attack, the aerodynamic force acting aft of the CG produces a restoring moment that tends to return the rocket toward alignment with its flight direction [1].

When the CP is forward of the CG, the aerodynamic moment is destabilizing: a small disturbance tends to increase rather than decrease [1].

### 3.4 Static Margin

Static margin is the signed axial separation between the CG and CP divided by a reference rocket diameter:

```text
Static Margin = signed CG-to-CP axial separation / reference diameter
```

The sign convention used by the application must produce:

- A **positive** value when the CP is aft of the CG.
- A value of **zero** when the CG and CP coincide.
- A **negative** value when the CP is forward of the CG.

Static margin is reported in **calibers**, where one caliber is one reference body diameter [3][8].

For the simplified single-main-body configurations supported by version 0.1, the main body diameter will be the reference diameter. If later versions support rockets with multiple substantially different body diameters, the reference-diameter rule must be reviewed and documented explicitly.

---

## 4. Research Findings by Static-Margin Range

### 4.1 Static Margin Less Than or Equal to 0 Calibers

#### Engineering interpretation

A static margin of zero or less does not demonstrate positive initial static stability.

- At exactly `0 calibers`, the CG and CP coincide under the calculation model. The initial aerodynamic moment does not provide a positive restoring lever arm in the linear static interpretation.
- Below `0 calibers`, the CP is forward of the CG. A disturbance can produce a destabilizing moment rather than a restoring moment [1].

RocketPy issues an `UnstableRocketWarning` when the static margin is negative at motor ignition and describes the configuration as aerodynamically unstable [5]. Although RocketPy's automatic warning is triggered for negative values, the MVP should include exactly zero in the same critical-review range because zero still does not demonstrate a positive restoring margin.

#### MVP determination

- **Meets intended static-margin criterion:** No
- **Requires additional review:** Yes
- **Demonstrates positive initial static stability:** No
- **Recommended finding category:** Warning
- **Recommended severity:** Critical Review

#### Basis for the recommendation

This is the highest-priority static-margin condition because the model does not show a positive initial restoring margin. The result should stop the user from interpreting the simulation as an acceptable initial static-stability condition, while still avoiding a claim that the tool has performed a complete safety evaluation.

---

### 4.2 Static Margin Greater Than 0 but Less Than 1 Caliber

#### Engineering interpretation

A result in this range places the CP aft of the CG, so the configuration has a positive initial restoring tendency under the static model. However, the separation is smaller than the common one-caliber design guideline used for typical subsonic model and high-power rockets [7][9].

The positive sign alone does not account for uncertainty in:

- The measured CG.
- The calculated CP.
- Component placement.
- Construction tolerances.
- Changes in CG and CP during flight.
- Wind and angle of attack at rail departure.
- Rotational inertia and aerodynamic damping.

Because the margin is positive but below the intended MVP range, the result is best treated as **marginal and requiring review**, not as equivalent to a negative static margin.

#### MVP determination

- **Meets intended static-margin criterion:** No
- **Requires additional review:** Yes
- **Demonstrates positive initial static stability:** Yes, within the assumptions of the static model
- **Recommended finding category:** Warning
- **Recommended severity:** Review Recommended

#### Basis for the recommendation

The distinction between this range and `SM <= 0` is important. A value between zero and one caliber is not the same as a configuration with a forward CP, but it provides less margin against modeling uncertainty and changing flight conditions than the intended MVP criterion.

---

### 4.3 Static Margin From 1 to 2 Calibers

#### Engineering interpretation

This range provides positive initial static stability and satisfies the intended simplified criterion selected for version 0.1.

A one-caliber separation is a widely used practical rule of thumb for typical subsonic rockets [7][9]. The upper part of the range provides additional separation between CG and CP without automatically treating the design as excessively stable.

OpenRocket documentation recommends not less than one caliber for subsonic flights and not less than two calibers for transonic and supersonic flights [7]. This means the `1–2 caliber` rule cannot be treated as a universal requirement for every rocket geometry and speed regime. It is appropriate for the MVP only as a simplified initial design-review criterion, subject to the limitations documented in this file.

#### MVP determination

- **Meets intended static-margin criterion:** Yes
- **Requires additional review because of this threshold alone:** No
- **Demonstrates positive initial static stability:** Yes, within the assumptions of the static model
- **Recommended finding category:** Passed
- **Recommended severity:** Not applicable

#### Basis for the recommendation

The research supports retaining `1 to 2 calibers`, inclusive, as the intended initial static-margin range for version 0.1. A Passed finding must mean only that this one documented criterion is satisfied. It must not state or imply that the complete rocket is safe or ready for flight.

Designs expected to enter transonic or supersonic flight require additional aerodynamic consideration. Version 0.1 may display maximum Mach number as information, but advanced speed-regime-specific stability diagnosis remains outside the current MVP unless separately researched and validated.

---

### 4.4 Static Margin Greater Than 2 Calibers

#### Engineering interpretation

A static margin greater than two calibers still indicates that the CP is aft of the CG. It does **not** represent a lack of positive initial static stability, and it must not automatically be described as unsafe.

A larger CG-to-CP separation generally produces a larger restoring moment for the same small disturbance. A strongly stable rocket can also respond more aggressively to crosswind and turn into the wind, a behavior known as weathercocking. Weathercocking can reduce altitude and alter the trajectory [2]. RocketPy documentation similarly warns that an *unreasonably high* static margin can indicate a super-stable configuration [6].

However, the reviewed sources do not establish `2.0 calibers` as a universal boundary at which a rocket becomes dangerously overstable. In fact, guidance may call for at least two calibers in transonic and supersonic applications [7]. Therefore, the MVP's `SM > 2` rule must be understood as a conservative **additional-review trigger**, not proof of overstability or unsafe flight.

#### MVP determination

- **Meets intended static-margin criterion:** No, because it is outside the selected `1–2 caliber` product range
- **Requires additional review:** Yes
- **Demonstrates positive initial static stability:** Yes, within the assumptions of the static model
- **Recommended finding category:** Warning
- **Recommended severity:** Review Recommended

#### Basis for the recommendation

The current Warning classification may be retained for version 0.1 because the dashboard is intended to identify results outside its documented target range. The implementation must clearly distinguish between:

- Being outside the MVP's intended range.
- Being proven dynamically overstable.
- Being unsafe to fly.

Only the first conclusion is supported directly by the threshold itself.

---

## 5. Recommended MVP Classification

| Initial Static-Margin Range | Engineering Interpretation | Meets MVP Criterion | Finding Category | Warning Severity |
| :--- | :--- | :---: | :--- | :--- |
| `SM <= 0` | Does not demonstrate positive initial static stability | No | Warning | Critical Review |
| `0 < SM < 1` | Positive but marginal initial static stability | No | Warning | Review Recommended |
| `1 <= SM <= 2` | Positive initial static stability within the intended MVP range | Yes | Passed | N/A |
| `SM > 2` | Positive initial static stability outside the intended MVP range | No | Warning | Review Recommended |

### Classification rules

1. The boundaries are inclusive exactly as shown in the table.
2. A value of exactly `0` belongs to the Critical Review range.
3. Values of exactly `1` and exactly `2` belong to the Passed range.
4. A value greater than `2` is not automatically unsafe and is not proof of dynamic overstability.
5. A Passed finding applies only to the initial static-margin criterion.
6. The final diagnostic wording will be defined in a separate issue.

---

## 6. Validation of the Intended 1–2 Caliber Range

### Decision

**Retain the provisional `1–2 caliber` range as the intended initial static-margin criterion for MVP version 0.1.**

### Engineering justification

The range is suitable for the MVP because:

- It requires a positive CG-to-CP separation.
- Its lower boundary agrees with the commonly used one-caliber guideline for typical subsonic rockets [7][9].
- It provides a simple and understandable first-stage diagnostic for the MVP's target users.
- It supports the product goal of identifying designs that deserve review without claiming complete safety certification.

### Important qualification

The range is a **design-review heuristic**, not a universal law or complete flight-safety criterion.

The OpenRocket recommendation of at least two calibers for transonic and supersonic flight shows that speed regime matters [7]. Practical research also indicates that the one-caliber rule may be insufficient for unusual geometries or larger angles of attack [10]. Therefore, the MVP must communicate its scope and limitations even when a design receives a Passed finding.

### Effect on the existing MVP document

The research does **not** require changing the four provisional numeric ranges currently listed in the MVP.

The following interpretation must remain attached to those ranges during implementation:

- `1–2 calibers` is the intended MVP criterion, not proof of overall safety.
- `SM > 2` is a conservative review condition, not an automatic unsafe or overstable determination.
- The classification applies to the initial static margin, not automatically to every point in the trajectory.
- Speed-regime-specific and dynamic-stability conclusions require separate validation.

---

## 7. Warning-Severity Rationale

### 7.1 Critical Review

Use **Critical Review** only for `SM <= 0` because the initial configuration does not demonstrate a positive restoring margin.

This label indicates that the static-stability result should receive immediate engineering attention before the result is relied upon. It remains a severity label within the Warning category and is not a separate diagnostic type.

### 7.2 Review Recommended

Use **Review Recommended** for:

- `0 < SM < 1`, because the configuration has a positive but marginal initial static margin.
- `SM > 2`, because the value is outside the intended MVP range and may warrant evaluation of wind response, trajectory behavior, mass distribution, and the applicability of the selected threshold.

These two ranges have the same warning severity but different engineering meanings. Their final messages must not imply that a value below one caliber and a value above two calibers represent the same physical condition.

---

## 8. Limitations of Static Margin as a Design Indicator

### 8.1 Static margin does not prove dynamic stability

Static margin indicates the initial direction of the aerodynamic restoring tendency under the assumptions of the model. It does not fully describe how quickly the rocket rotates, whether oscillations decay, or whether the rocket will maintain a satisfactory attitude throughout flight.

Rotational inertia, aerodynamic damping, angular rates, and changing forces affect dynamic response. OpenRocket documentation specifically notes that rotational inertia can materially affect whether a simulated rocket behaves as stable or unstable near recommended margins [7].

### 8.2 CG and CP can change during flight

The CG changes as propellant burns, and the CP can change with Mach number and aerodynamic conditions. RocketPy's flight stability margin accounts for both Mach-dependent CP variation and propellant-dependent CG variation [3][4]. An acceptable initial value therefore does not guarantee an acceptable value at every time in the trajectory.

### 8.3 Wind and rail-departure conditions matter

Crosswind creates an effective angle of attack and can cause a stable rocket to turn into the wind. The resulting weathercocking can reduce altitude and alter the flight path [2]. Static margin alone does not determine whether rail-exit velocity, wind, launch-rail length, or launch angle are suitable.

### 8.4 The one-caliber guideline is not universal

The common one-caliber rule is a practical guideline rather than a universal boundary [9]. OpenRocket recommends a larger minimum for transonic and supersonic flights [7], and practical aerodynamic research shows that geometry and angle of attack can make a single fixed threshold insufficient [10].

### 8.5 Input and model accuracy directly affect the result

The classification is only as reliable as the inputs and aerodynamic model. Errors in the following can change the result:

- Rocket mass.
- Installed-motor mass and propellant data.
- Measured CG position.
- Fin geometry and placement.
- Nose-cone geometry and placement.
- Body diameter used as the caliber reference.
- Coordinate-system orientation.
- Component positions.

RocketPy advises users to verify component positions and warns that the static margin becomes unreliable when unsupported aerodynamic-surface contributions are omitted from the CP calculation [5][6].

### 8.6 Simplified aerodynamic assumptions may not cover every geometry

Static CP methods are generally most dependable for the geometries and operating conditions for which their assumptions were developed. Unusual rockets, large changes in body diameter, nonstandard aerodynamic surfaces, high angles of attack, and complex multi-stage configurations may require more advanced methods or independent validation.

### 8.7 Static margin is not a complete safety evaluation

Static margin does not evaluate all flight risks. It does not independently verify:

- Structural strength or fin flutter.
- Motor or recovery-system suitability.
- Rail-guide loads.
- Deployment timing and descent safety.
- Maximum aerodynamic loads.
- Launch-site and regulatory compliance.
- Manufacturing quality or damage.
- Uncertainty caused by atmospheric variation.

The dashboard must therefore present static margin as one design indicator within a broader engineering review.

---

## 9. Provisional Conclusions and Future Validation

The following conclusions remain provisional or require additional work beyond this issue:

1. **Exact upper threshold for overstability:** The research does not establish `2 calibers` as a universal onset of excessive stability. Future work may determine whether the `SM > 2` finding should remain a Warning, become Information, or depend on other simulated conditions.
2. **Mach-dependent criteria:** OpenRocket recommends different minimum margins for subsonic versus transonic and supersonic flights [7]. A future issue should determine whether the dashboard should use speed-conditioned thresholds.
3. **Time-varying stability findings:** RocketPy can provide stability margin during flight [3][4]. Separate research is needed before creating findings based on minimum, maximum, rail-exit, or time-dependent stability margins.
4. **Uncertainty treatment:** Future work should determine whether measurement tolerances for CG, mass, diameter, and component positions should be propagated into a static-margin uncertainty band.
5. **Supported geometry limits:** The implementation must validate that the selected RocketPy aerodynamic components and CP calculations are appropriate for the geometries accepted by the form.
6. **Reference-diameter convention:** The main body diameter is acceptable for the simplified version 0.1 configuration. Multi-diameter and clustered configurations require an explicit future convention.
7. **Dynamic behavior:** A rocket may satisfy the static-margin criterion and still exhibit undesirable dynamic behavior. Dynamic-stability diagnostics remain outside the core MVP until separately researched and validated.

These provisional items do not prevent implementation of the initial four-range classification, provided the dashboard communicates the limitations described in this document.

---

## 10. Conclusion Traceability

| Conclusion | Supporting References |
| :--- | :--- |
| Positive static stability requires the CP to be aft of the CG | [1] |
| A negative initial static margin is treated by RocketPy as aerodynamically unstable | [5] |
| Static margin is measured in calibers based on rocket diameter | [3], [8] |
| One caliber is a common practical minimum for typical subsonic rockets | [7], [9] |
| Transonic and supersonic flights may require at least two calibers | [7] |
| CG and CP can change during flight | [3], [4], [7] |
| Rotational inertia affects stability behavior beyond the static-margin number | [7] |
| Weathercocking can alter trajectory and reduce altitude | [2] |
| An unreasonably high margin may indicate a super-stable RocketPy configuration | [6] |
| A fixed one-caliber rule may be insufficient for some geometries and angles of attack | [10] |
| The four MVP ranges may be retained with documented limitations | Synthesis of [1]–[10] |

---

## 11. Final Recommendation

Version 0.1 should implement the following initial static-margin classification:

- `SM <= 0`: **Warning — Critical Review**
- `0 < SM < 1`: **Warning — Review Recommended**
- `1 <= SM <= 2`: **Passed**
- `SM > 2`: **Warning — Review Recommended**

The thresholds should be implemented exactly at the documented boundaries. The dashboard must also preserve the following engineering meaning:

- Only `SM <= 0` represents a lack of positive initial static stability.
- Both `0 < SM < 1` and `SM > 2` require review for different reasons.
- `1 <= SM <= 2` satisfies the selected MVP criterion but does not certify the rocket as safe.
- `SM > 2` must not automatically be described as unsafe or conclusively overstable.
- The result applies to the initial static configuration and does not replace complete trajectory, dynamic-stability, or safety evaluation.

No final user-facing diagnostic wording is defined in this document.

---

## 12. References

Accessed July 29, 2026.

1. **NASA Glenn Research Center.** “Rocket Stability Condition.” Explains the restoring and destabilizing moments created by the relative CG and CP positions.  
   https://www1.grc.nasa.gov/beginners-guide-to-aeronautics/conditions-for-rocket-stability/

2. **NASA Glenn Research Center.** “Effects of Weathercocking.” Explains how aerodynamic side force causes a stable rocket to turn into the wind and how this can reduce maximum altitude.  
   https://www1.grc.nasa.gov/beginners-guide-to-aeronautics/effects-of-weathercocking/

3. **RocketPy Documentation, Version 1.13.0.** “Rocket Class.” Defines static margin and stability margin in calibers and documents Mach- and time-dependent stability calculations.  
   https://docs.rocketpy.org/en/latest/reference/classes/Rocket.html

4. **RocketPy Documentation, Version 1.13.0.** “Flight Class.” Documents the flight stability margin and its consideration of Mach-dependent CP and propellant-dependent CG variation.  
   https://docs.rocketpy.org/en/latest/reference/classes/Flight.html

5. **RocketPy Documentation, Version 1.13.0.** “Exceptions and Warnings.” Documents `UnstableRocketWarning` for a negative static margin at motor ignition and the `GenericSurface` limitation.  
   https://docs.rocketpy.org/en/latest/reference/classes/exceptions.html

6. **RocketPy Documentation, Version 1.13.0.** “First Simulation with RocketPy.” Advises checking static margin, describes negative margin as unstable, and cautions against unreasonably high static margin.  
   https://docs.rocketpy.org/en/latest/user/first_simulation.html

7. **OpenRocket Documentation, Version 23.09.** “Overrides and Surface Finish.” Recommends not less than one caliber for subsonic flights and not less than two calibers for transonic and supersonic flights; also discusses time-varying margin and rotational inertia.  
   https://openrocket.readthedocs.io/en/latest/user_guide/overrides_and_surface_finish.html

8. **OpenRocket Documentation, Version 23.09.** “Preferences.” Defines caliber as one rocket diameter for stability-margin units.  
   https://openrocket.readthedocs.io/en/latest/setup/preferences.html

9. **Apogee Components.** “Peak of Flight Newsletter No. 594.” Describes the commonly used one-caliber rule of thumb and explains that CG and CP can shift during flight.  
   https://www.apogeerockets.com/Peak-of-Flight/Newsletter594

10. **Galejs, Robert J., via Apogee Components.** “What Barrowman Left Out,” Peak of Flight Newsletter No. 470. Discusses CP movement with angle of attack and examples where the one-caliber rule may be insufficient for particular geometries.  
    https://www.apogeerockets.com/Peak-of-Flight/Newsletter470
