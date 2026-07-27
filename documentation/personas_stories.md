# User Personas and User Stories

## Overview

These personas represent the primary users of the Flight Performance Analyzer Tool. They guide the design of the rocket-input workflow, RocketPy simulation, results dashboard, stability diagnostics, and design comparison features.

---

## 1. Alex Rivera — Aerospace Engineering Student

### Background and Role

Alex is a university student learning rocket design and flight dynamics. Alex understands basic engineering concepts but has limited experience interpreting RocketPy results.

### Needs

* A guided process for entering rocket and environmental data.
* Clear explanations of performance and stability metrics.
* Visual identification of the center of gravity (CG) and center of pressure (CP).

### Goals

* Understand how design choices affect predicted flight behavior.
* Complete simulations without manually programming RocketPy.
* Use simulation results in assignments and design reviews.

### Frustrations

* Simulation outputs contain unfamiliar terminology.
* Small input errors can produce confusing results.
* Static and dynamic stability are easy to misunderstand.

### Primary Use Case

Define a class rocket, run a simulation, and review its predicted performance and static margin.

---

## 2. Daniel Morales — High-Power Rocket Hobbyist

### Background and Role

Daniel designs and builds high-power rockets as a hobby. He understands practical construction but wants additional support interpreting simulation results before testing a design.

### Needs

* Efficient rocket-configuration input or import.
* Clear performance and stability diagnostics.
* Warnings about questionable inputs or results.

### Goals

* Identify design parameters that require further investigation.
* Compare predicted performance before and after a modification.
* Review important results without searching through raw simulation data.

### Frustrations

* Simulation tools often present too much raw information.
* Comparing design revisions manually is time-consuming.
* A good static margin can hide other possible concerns.

### Primary Use Case

Modify fin geometry or component placement and compare the revised simulation with the original design.

---

## 3. Sofia Santiago — University Rocket Team Member

### Background and Role

Sofia belongs to a university rocketry team and helps evaluate competing design revisions. She needs consistent evidence for team design decisions.

### Needs

* Repeatable simulations using documented inputs.
* Consistent metrics and visualizations across revisions.
* Traceable diagnostics connected to calculations or simulation outputs.

### Goals

* Compare design alternatives objectively.
* Communicate findings during team design reviews.
* Detect performance or stability changes caused by design revisions.

### Frustrations

* Team members may use inconsistent assumptions.
* Results from different simulations are difficult to compare.
* Important warnings can become buried in technical data.

### Primary Use Case

Run multiple configurations and compare altitude, velocity, acceleration, static margin, and other supported indicators.

---

## 4. Dr. Elena Cruz — Instructor or Team Mentor

### Background and Role

Dr. Cruz reviews preliminary rocket designs and teaches students how to interpret engineering results. She does not treat simulations as safety certification.

### Needs

* Organized summaries of inputs, assumptions, and results.
* Diagnostics supported by calculations or documented rules.
* Clear communication of model limitations and uncertainty.

### Goals

* Review student designs efficiently.
* Verify that conclusions are supported by simulation evidence.
* Help users distinguish preliminary analysis from flight approval.

### Frustrations

* Users may present conclusions without supporting evidence.
* Assumptions and limitations are often undocumented.
* Simulation results may be mistaken for guaranteed real-flight behavior.

### Primary Use Case

Review a team’s configuration, results, diagnostics, and design comparison during a preliminary design review.

---

# User Stories

| ID    | User Story                                                                                                                                    | Relevant Feature            | Real App Use Case                                                                                          |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------- | ---------------------------------------------------------------------------------------------------------- |
| US-01 | As a user, I want to enter or import a rocket configuration so that I can analyze a design without manually creating a RocketPy workflow.     | Rocket input/import         | Alex enters the rocket’s dimensions, components, motor, mass properties, and environment.                  |
| US-02 | As a user, I want the system to validate my inputs so that I can correct missing or unsupported values before simulation.                     | Input validation            | Daniel receives an error when required fin geometry or CG information is missing.                          |
| US-03 | As a user, I want to run a RocketPy simulation from the dashboard so that I can evaluate predicted flight performance.                        | Simulation execution        | Sofia submits a valid configuration and starts a simulation.                                               |
| US-04 | As a user, I want to see key results in an organized dashboard so that I do not have to interpret only raw numerical output.                  | Results dashboard           | Alex reviews predicted altitude, velocity, acceleration, and flight events.                                |
| US-05 | As a user, I want to see the CG, CP, and static margin so that I can evaluate preliminary static stability.                                   | Static-stability analysis   | Daniel reviews a calculated static margin of 1.61 calibers and sees CG and CP on a side-view diagram.      |
| US-06 | As a user, I want technical metrics explained in understandable language so that I can learn what the results mean.                           | Metric explanations         | Alex opens the static-margin explanation and learns that a positive value means the CP is behind the CG.   |
| US-07 | As a user, I want questionable results highlighted so that I know which design areas require further investigation.                           | Diagnostics and warnings    | Sofia receives a warning when the CP is ahead of the CG or the simulation enters an unsupported condition. |
| US-08 | As a user, I want each diagnostic connected to its source result or calculation so that I can verify the conclusion.                          | Traceable diagnostics       | Dr. Cruz traces a stability warning to the calculated CP, CG, and static-margin values.                    |
| US-09 | As a user, I want to modify a design and rerun the analysis so that I can evaluate the effect of a change.                                    | Design revision             | Daniel changes the fin span and runs a new simulation.                                                     |
| US-10 | As a user, I want to compare simulation revisions using consistent metrics so that I can choose between design alternatives.                  | Simulation comparison       | Sofia compares two designs by altitude, velocity, acceleration, and stability indicators.                  |
| US-11 | As a user, I want assumptions, uncertainty, and model limitations displayed so that I do not treat the results as guaranteed flight behavior. | Limitations and uncertainty | Dr. Cruz confirms that the analysis is presented as design support rather than safety certification.       |
| US-12 | As a mentor, I want a clear summary of configuration inputs and results so that I can review a preliminary design efficiently.                | Design-review summary       | Dr. Cruz examines the rocket configuration, simulation results, diagnostics, and limitations in one view.  |

---

## Product Boundary

The tool supports education and engineering design decisions. Its simulations and diagnostics do not certify a rocket as safe or legally approved to fly, and they do not replace physical testing, experienced engineering review, or range-safety procedures.
