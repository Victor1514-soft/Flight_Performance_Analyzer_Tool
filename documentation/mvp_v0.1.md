# Rocket Design Diagnostic Dashboard: MVP Definition (Version 0.1)

## 1. MVP Summary

Version 0.1 of the Rocket Design Diagnostic Dashboard will provide a preliminary, simulation-based assessment of a high-powered rocket design. The user will manually enter the minimum rocket geometry, mass, motor, and launch-condition information required by the application. The system will validate the inputs, run a RocketPy simulation, calculate the main static-stability values, and display the results in a dashboard containing key flight metrics, charts, and diagnostic findings. The diagnostic will help the user identify possible design concerns and understand which areas of the design should be reviewed. It will not automatically redesign the rocket, provide exact dimensional corrections, certify the rocket as safe, or approve it for flight. The primary target users are members of university rocketry teams and high-powered rocket designers with basic or intermediate knowledge. The results should still be written clearly enough to be useful to users with different experience levels.

---

## 2. Primary User Need

The MVP should help a user answer the following question:

> Does this rocket design show apparent static-stability or simulation-related concerns, and which parts of the design should be reviewed before continuing with more advanced analysis, testing, or construction?

The application should not only display numerical results. It should also explain important findings in understandable language and identify possible areas to inspect, such as:

* Center-of-gravity position
* Fin geometry
* Mass distribution
* Rocket geometry
* Launch conditions
* Missing or invalid simulation inputs

The MVP will not tell the user exactly how many centimeters to move a component or how to optimize the design.

---

## 3. Primary User Flow

1. The user opens the landing page.
2. The user reviews the purpose, limitations, and safety disclaimer of the application.
3. The user selects **Create New Analysis**.
4. The user selects the preferred measurement system and enters the required rocket-design information in a manual form.
5. The user selects one motor from a small set of preconfigured motors.
6. The user reviews the default simulation conditions and changes the configurable values when necessary.
7. The application validates the provided information.
8. If validation fails, the application displays clear error messages and identifies the fields that must be corrected.
9. The user submits the design for analysis.
10. The backend builds and executes the RocketPy simulation.
11. If the simulation fails, the application displays an error finding with an understandable explanation.
12. If the simulation succeeds, the application displays the results dashboard.
13. The dashboard presents:

    * Static-stability metrics
    * Main flight-performance metrics
    * Time-based charts
    * A two-dimensional trajectory view
    * Diagnostic findings
14. The user reviews the findings and identifies which parts of the design may require additional investigation.
15. The user returns to the form with the previous values preserved, modifies the design, and runs another analysis during the current session.

Version 0.1 will not permanently save or compare separate analyses.

---

## 4. Minimum Rocket Design Information

The user must manually provide enough information to construct a valid simplified RocketPy model.

### 4.1 General Rocket Information

* Design name
* Preferred measurement system: SI or U.S. customary units
* Total rocket length
* Main body diameter
* Main body length

### 4.2 Nose Cone

* Nose-cone type or geometry
* Nose-cone length
* Nose-cone position, when required by the model

Only nose-cone geometries supported and validated by the implementation will be available.

### 4.3 Mass Properties

* Total rocket mass
* Center-of-gravity position measured from a clearly defined reference point

Version 0.1 will not calculate the center of gravity from individual components. The interface must explain how the CG reference position should be measured.

### 4.4 Fin Set

* Number of fins
* Root chord
* Tip chord
* Fin span
* Sweep length or another equivalent parameter supported by the selected fin model
* Fin position along the rocket body
* Fin thickness, if required by the implemented aerodynamic model

The MVP will support one primary fin set. Multiple independent fin sets are outside the initial scope unless they are required by the final validated RocketPy implementation.

### 4.5 Motor

* Selection of one motor from a small preconfigured list

The initial list will contain approximately three to five motors. The specific motors will be selected during implementation and testing.

### 4.6 Required Input Validation

The application must detect at least:

* Missing required values
* Non-numeric values in numeric fields
* Zero or negative dimensions where they are not physically valid
* A center-of-gravity position outside the defined rocket length
* Invalid fin dimensions
* Unsupported nose-cone or motor selections
* Missing or unsupported measurement-system selections
* Values that prevent the RocketPy model from being created
* Simulation execution failures

### 4.7 Units and Conversion Strategy

Version 0.1 will use **SI units as the canonical internal measurement system** for calculations, simulation inputs, stored application state, and comparisons performed during the current session.

The user may select one of the following measurement systems for entering and viewing values:

* **SI units**
* **U.S. customary units**

When the user selects U.S. customary units, the application will automatically convert the entered values to SI before constructing the RocketPy model. Simulation results will be converted from SI to the selected display system before they are shown to the user. The user will not be required to perform manual conversions.

The MVP will use the following units:

| Quantity                                  | SI display                       | U.S. customary display          |
| :---------------------------------------- | :------------------------------- | :------------------------------ |
| Rocket dimensions and component positions | millimeters (mm)                 | inches (in)                     |
| Launch-site elevation and flight altitude | meters (m)                       | feet (ft)                       |
| Launch-rail length                        | meters (m)                       | feet (ft)                       |
| Mass                                      | kilograms (kg)                   | pounds (lb)                     |
| Velocity and wind speed                   | meters per second (m/s)          | feet per second (ft/s)          |
| Acceleration                              | meters per second squared (m/s²) | feet per second squared (ft/s²) |
| Time                                      | seconds (s)                      | seconds (s)                     |
| Angles                                    | degrees (°)                      | degrees (°)                     |
| Static margin                             | calibers                         | calibers                        |
| Mach number                               | dimensionless                    | dimensionless                   |

Every numerical input and displayed result must show its associated unit. Changing the selected display system must not change the underlying physical value. Rounding will be applied only to displayed values; calculations will use the unrounded internal SI values.

The final display precision and acceptable conversion tolerance will be documented before the unit-conversion feature is considered implementation-ready.

---

## 5. Simulation Inputs

Version 0.1 will use default simulation values so the user can run an analysis without configuring every environmental parameter.

The user may modify the following inputs:

* Launch-site elevation
* Launch-rail length
* Launch-rail inclination
* Wind speed
* Wind direction

The following values may use documented defaults in version 0.1:

* Atmospheric temperature
* Atmospheric pressure
* Other environmental or numerical parameters required by RocketPy

The interface must show which values are defaults. The selected default values must be documented in the repository and kept consistent during testing.

Advanced atmospheric profiles, live weather integrations, custom atmospheric files, and complete environmental configuration are outside version 0.1.

---

## 6. Meaning of “Rocket Design Diagnostic” in Version 0.1

Within version 0.1, a **rocket design diagnostic** is:

> A rule-based interpretation of validated simulation outputs that identifies input errors, possible static-stability concerns, successfully satisfied checks, and important informational flight results.

The diagnostic will:

* Validate whether the provided data can create a usable simulation model.
* Calculate and display the center of pressure.
* Use the user-provided center of gravity.
* Calculate the static margin in calibers.
* Classify the static margin using documented thresholds.
* Explain the meaning of important results.
* Identify general design areas that the user should review.
* Display key simulated flight metrics and charts.

The diagnostic will not:

* Automatically optimize the design.
* Generate exact numerical design corrections.
* Prove that the rocket is safe.
* Replace engineering review, physical testing, manufacturer instructions, applicable regulations, or launch-organization requirements.

---

## 7. Static-Stability Scope

Static-stability analysis is a required part of the core MVP.

### 7.1 Required Static-Stability Results

The dashboard must display:

* User-provided center of gravity (CG)
* Calculated center of pressure (CP)
* Static margin in calibers
* A clear status associated with the static margin
* A plain-language explanation of the result

Where the validated RocketPy implementation supports it, the dashboard should also display:

* Static margin during the simulated flight
* Static margin versus time

At minimum, version 0.1 must provide a validated static-margin value for the initial rocket configuration used by the simulation.

### 7.2 Provisional Static-Margin Interpretation

The following thresholds are provisional and must be verified during the static-stability research before implementation is considered complete:

* **Static margin less than or equal to 0 calibers:** Warning — Critical Review
* **Static margin greater than 0 but less than 1 caliber:** Warning — Review Recommended
* **Static margin from 1 to 2 calibers:** Passed
* **Static margin greater than 2 calibers:** Warning — Review Recommended

The terms **Critical Review** and **Review Recommended** are severity labels within the **Warning** category. They are not separate diagnostic finding types.

A result greater than 2 calibers must not automatically be described as unsafe. The wording should explain that the result is outside the intended range and may require further evaluation.

Examples:

> **Warning — Critical Review:** The static margin is zero or negative. The simulated configuration does not demonstrate positive initial static stability. Review the center-of-gravity position, center-of-pressure position, fin geometry, and mass distribution before continuing.

> **Warning — Review Recommended:** The static margin is greater than 2 calibers. The result is outside the intended range and should be evaluated further, but it does not by itself establish that the rocket is unsafe.

---

## 8. Dynamic-Stability Scope

Dynamic-stability analysis is not a required feature of the core MVP because:

* The initial research is not complete.
* The diagnostic method has not been defined.
* The team does not yet have RocketPy experience.
* The project must remain achievable by October 12.

A basic dynamic-stability diagnostic will remain a stretch goal.

No implementation deadline or inclusion decision is currently assigned to this stretch goal. Its status is **on hold** until the research identifies:

* The metrics that can be obtained or derived reliably
* The diagnostic rules that should be applied
* The validation method
* The estimated development effort
* Whether it can be completed without delaying the core MVP

Advanced dynamic-stability analysis remains outside version 0.1.

---

## 9. Main Metrics and Simulation Results

The results dashboard will display the following metrics when the simulation provides valid results:

* Maximum altitude
* Maximum velocity
* Maximum Mach number
* Maximum acceleration
* Time to apogee
* Total flight time
* Velocity at launch-rail exit
* Position or trajectory information

### 9.1 Maximum Mach Number

The Mach number compares the rocket's velocity with the local speed of sound. For example:

* Mach 0.5 is approximately half the speed of sound.
* Mach 1 is approximately the speed of sound.

In version 0.1, maximum Mach number may be presented as an informational result. Advanced aerodynamic diagnosis related to transonic or supersonic flight is outside the MVP unless supported by completed research.

### 9.2 Required Charts

The dashboard will include:

* Altitude versus time
* Velocity versus time
* Acceleration versus time
* Two-dimensional rocket trajectory or displacement view

A three-dimensional trajectory, geographic map, and landing-location prediction are outside version 0.1.

---

## 10. Diagnostic Finding Types

Every diagnostic message must use one of the following categories:

### Error

An error means that the analysis cannot be completed correctly.

Examples:

* A required value is missing.
* A dimension is invalid.
* The center of gravity is outside the rocket length.
* The simulation model cannot be created.
* RocketPy fails to complete the simulation.

### Warning

A warning means that the simulation completed, but the application identified a condition that may require review.

A warning may include a severity label, such as **Critical Review** or **Review Recommended**, to communicate urgency. These labels remain part of the **Warning** category and must not be presented as additional diagnostic finding types.

Example:

> **Warning — Review Recommended:** The static margin is below 1 caliber. Review the center-of-gravity position, fin geometry, or mass distribution.

### Passed

A passed finding means that a specific evaluated criterion is within the documented intended range.

Example:

> The initial static margin is between 1 and 2 calibers.

A passed finding does not certify the entire rocket as safe.

### Information

An informational finding presents a relevant result that is not automatically good or bad.

Examples:

* Maximum estimated velocity
* Maximum Mach number
* Time to apogee
* Default environmental conditions used

---

## 11. Features Included in the Core MVP

### Application and Interface

* Simple landing page
* Clear product description
* Safety and limitation disclaimer
* Manual rocket-design form
* SI or U.S. customary measurement-system selector
* Automatic conversion to the internal SI representation
* Explicit unit labels on numerical inputs and results
* Form validation
* Preconfigured motor selector
* Default simulation conditions
* Configurable launch and wind inputs
* Results dashboard
* Help or instructions section
* Responsive web layout
* Ability to return to the form and modify the current design

### Analysis

* RocketPy integration
* Simulation execution
* Center-of-pressure calculation
* Static-margin calculation
* Static-stability classification
* Rule-based diagnostic messages
* Main flight-performance metrics
* Required charts
* Two-dimensional trajectory display
* Understandable simulation-error reporting

### Responsive Layout Definition

A responsive layout means that the web interface automatically adapts to different screen sizes, including desktop computers, tablets, and mobile phones.

Version 0.1 does not require a separate mobile application. The same web application should remain readable and usable on supported smaller screens without major content overlap or horizontal scrolling.

---

## 12. Non-Goals and Features Excluded from Version 0.1

The following are explicitly outside the core MVP:

* Automatic design optimization
* Exact numerical design recommendations
* Three-dimensional rocket modeling
* Visual rocket editing
* Real-time telemetry
* Sensor integration
* OpenRocket import
* User collaboration
* Design sharing
* PDF report generation
* Safety certification
* Flight approval
* Confirmation of compliance with flight codes or regulations
* Structural analysis
* Thermal analysis
* Recovery-system design or parachute selection
* Landing-location prediction
* Geographic trajectory maps
* Extensive motor database
* Custom motor uploads
* Multiple-design comparison
* User registration
* Login and authentication
* User profiles
* Permanent simulation history
* Permanent cloud storage of designs
* Advanced atmospheric configuration
* Live weather integration
* Advanced dynamic-stability analysis

---

## 13. Stretch Goals

Stretch goals may only be started after all core MVP acceptance criteria are satisfied and the remaining schedule is reviewed.

The initial stretch goals are:

1. **Basic dynamic-stability diagnostic**

   * Only if the research defines validated metrics and rules that can be implemented without delaying the MVP.

2. **Automatic center-of-gravity calculation**

   * Calculate CG from user-entered component masses and positions.

3. **Save and review simulations**

   * Store designs and simulation results for later access.

Stretch goals are not required for version 0.1 approval and must not delay the core release.

---

## 14. Assumptions

The MVP is based on the following assumptions:

* The approved problem statement remains the main project reference.
* The personas and user stories have been reviewed and confirmed to be consistent with the MVP scope.
* Static- and dynamic-stability research is still in progress.
* Static-margin thresholds will be validated before diagnostic rules are finalized.
* The MVP will use a simplified rocket representation supported by the selected RocketPy model.
* The user can provide a reasonable total mass and center-of-gravity position.
* A small set of valid motors can be preconfigured.
* Default atmospheric conditions are acceptable for preliminary analysis.
* The team consists of two developers.
* Both team members will contribute code.
* The team has prior experience with React and Python.
* The team does not yet have experience with RocketPy.
* No initial codebase exists.
* Each team member expects to dedicate approximately two hours per day.
* October 12 is the worst-case target date for the core MVP.

---

## 15. Limitations

Version 0.1 will have the following limitations:

* Results depend on the accuracy of user-entered data.
* The center of gravity is supplied by the user rather than calculated from components.
* The motor list is intentionally small.
* Environmental modeling is simplified through defaults.
* The diagnostic uses rule-based interpretations rather than automatic engineering optimization.
* Static-margin rules remain subject to research validation.
* Dynamic-stability diagnosis is not guaranteed to be included.
* The application does not validate manufacturing quality.
* The application does not model every physical failure mode.
* The simulation cannot replace ground testing or professional engineering review.
* Results may not apply to rocket geometries or operating conditions outside the validated implementation.

---

## 16. Safety and Certification Disclaimer

> This application provides preliminary, simulation-based design analysis for educational and early design-review purposes. It does not certify that a rocket is safe, structurally sound, compliant with regulations, approved by a launch organization, or ready for flight. Users remain responsible for verifying all design assumptions, following motor and component manufacturer instructions, performing appropriate testing, consulting qualified reviewers, and complying with all applicable laws, regulations, safety codes, and launch-site requirements.

This disclaimer must be visible on the landing page and accessible from the results dashboard.

---

## 17. Unresolved Decisions

The following decisions remain open but do not prevent approval of the core MVP scope:

* Completion of static-stability research
* Completion of dynamic-stability research
* Final static-margin diagnostic wording
* Final display precision and acceptable conversion tolerance
* Final selection of three to five preconfigured motors
* Final default atmospheric values
* Exact RocketPy model and supported nose-cone and fin configurations
* Whether time-varying static margin can be reliably included
* Whether any stretch goal can be attempted
* Dynamic-stability inclusion decision, currently on hold

Any unresolved decision that affects a core feature must be resolved before that feature is considered implementation-ready.

---

## 18. Core MVP Acceptance Criteria

Version 0.1 will be considered complete only when all applicable core acceptance criteria below are satisfied. Stretch goals are not required for MVP approval.

### 18.1 Landing Page and User Guidance

* The landing page explains the purpose of the application and identifies it as a preliminary, simulation-based design-analysis tool.
* The safety and certification disclaimer is visible on the landing page and accessible from the results dashboard.
*  The interface provides enough guidance for the user to understand the required inputs, the center-of-gravity reference point, and the meaning of the displayed units.

### 18.2 Rocket Input and Unit Handling

*  The form includes all required rocket geometry, mass-property, motor, and launch-condition inputs defined in this document.
*  The user can select either SI or U.S. customary units for data entry and result display.
*  Every numerical input and displayed result shows its associated unit.
*  Values entered in U.S. customary units are converted to the canonical internal SI representation before the RocketPy model is constructed.
*  Switching the display system changes only the displayed values and units, not the underlying physical quantities.
*  Unit-conversion tests satisfy the documented conversion tolerance.
*  Display rounding does not alter the unrounded values used for calculations or simulation execution.

### 18.3 Validation and Simulation Execution

*  Missing, non-numeric, unsupported, or physically invalid required inputs are detected before simulation execution.
*  Validation errors identify the affected fields and explain what must be corrected.
*  A valid supported configuration can be converted into a RocketPy model and executed successfully.
*  A RocketPy model-construction or simulation-execution failure produces an understandable **Error** finding rather than an incomplete or misleading dashboard.
*  The default simulation values used by the application are visible to the user and documented in the repository.

### 18.4 Static-Stability Results

*  The dashboard displays the user-provided center of gravity, calculated center of pressure, and initial static margin in calibers.
*  The initial static-margin calculation is validated against at least one documented reference configuration or independently verified calculation.
*  The displayed static-margin status follows the final validated thresholds approved by the static-stability research.
*  A static margin between 1 and 2 calibers produces a **Passed** finding without certifying the complete rocket as safe.
*  A static margin outside the intended range produces a **Warning** with the appropriate severity label and review guidance.
*  No static-margin result is described as proof that the rocket is safe, approved, or ready for flight.

### 18.5 Flight Results and Visualizations

*  A successful simulation displays maximum altitude, maximum velocity, maximum Mach number, maximum acceleration, time to apogee, total flight time, velocity at launch-rail exit, and position or trajectory information when valid results are available.
*  The dashboard includes altitude-versus-time, velocity-versus-time, and acceleration-versus-time charts.
*  The dashboard includes a two-dimensional trajectory or displacement view.
*  Each chart has a clear title, labeled axes, and visible units.
*  Informational metrics are not automatically presented as positive or negative findings unless a documented diagnostic rule applies.

### 18.6 Diagnostic Findings

*  Every diagnostic message uses exactly one of the four approved categories: **Error**, **Warning**, **Passed**, or **Information**.
*  Warning severity labels, including **Critical Review** and **Review Recommended**, are presented as labels within the **Warning** category rather than as separate categories.
*  Each finding identifies the relevant result or input and provides a clear explanation appropriate for the target users.
*  Rule-based findings can be traced to a documented threshold, validation rule, simulation output, or calculation.
*  Diagnostic findings recommend areas for review without generating exact dimensional corrections or automatically redesigning the rocket.

### 18.7 Session Workflow and Responsive Interface

*  After reviewing the dashboard, the user can return to the form with the current values preserved, modify the design, and run another analysis during the same session.
*  Version 0.1 does not require login, permanent cloud storage, permanent simulation history, or comparison of separately saved analyses.
*  The application remains readable and usable on supported desktop, tablet, and mobile screen sizes without major content overlap or unintended horizontal scrolling.

### 18.8 Documentation and Release Approval

*  The repository documents the supported rocket model, supported nose-cone and fin configurations, preconfigured motors, default simulation values, measurement units, conversion rules, display precision, and diagnostic thresholds.
* Known assumptions, unsupported configurations, and model limitations are documented and visible where relevant.
*  All unresolved decisions affecting a core feature are resolved before that feature is marked implementation-ready.
*  All core acceptance criteria are tested and satisfied before work begins on stretch goals.
