# Barrowman Equations, Summary & Worked Example

## 1. What They Are 

The Barrowman equations estimate the aerodynamic behavior of slender, finned rockets by evaluating the nose, body, fins, and fin-body interference separately, then combining them.

**Main outputs:**
- **$C_{N_\alpha}$**, normal-force coefficient derivative (how much aerodynamic force builds per degree of angle of attack)
- **$x_{CP}$**, center of pressure, the effective point where total aerodynamic force acts
- **Static margin ($SM$)**, the CP-to-CG separation, in body diameters ("calibers")

**Why it matters:** A rocket is statically stable when the CG sits ahead of the CP,

$$SM = \frac{x_{CP}-x_{CG}}{D}, \quad SM > 0 \implies \text{stable}$$

A positive margin means a disturbance produces a *restoring* moment rather than a divergent one. Typical rule of thumb: **1–2 calibers** is a healthy design target. This is a **static**-stability result only; it says nothing about whether oscillations grow or decay over time (that needs velocity, mass, and damping data), and it assumes a rigid vehicle, sharp nose, and near-zero angle of attack, treat results as preliminary estimates, not certification.

## 2. Input Checklist

| Category | Needed |
|---|---|
| Body | Diameter $D$, radius $R=D/2$ |
| Nose | Shape, length $L_n$ |
| Fins | Count $N$, root chord $c_r$, tip chord $c_t$, span $s$, leading-edge position $x_{LE}$, sweep offset $x_R$ |
| Mass | CG location $x_{CG}$ (from nose tip) |
| Other | One consistent unit system throughout |

*(Velocity, air density, mass, and inertia are only needed for dynamic-stability work, out of scope here.)*

## 3. Worked Example

**Geometry:** conical nose, 4 unswept rectangular fins.

| Parameter | Value |
|---|---|
| $D$ | 0.100 m |
| $L_n$ | 0.300 m |
| $N$ | 4 |
| $c_r = c_t$ | 0.150 m |
| $s$ | 0.100 m |
| $x_{LE}$ | 0.900 m |
| $x_{CG}$ | 0.650 m |

**Step 1, Nose:** For a sharp cone, 

$C_{N_{\alpha,n}} = 2$, with CP at $x_n = \tfrac{2}{3}L_n = 0.200$ m.

**Step 2, Fins:** Using Barrowman's subsonic fin normal-force and CP formulas:

$$C_{N_{\alpha,f}} \approx 9.689, \qquad x_f = x_{LE} + \frac{c}{4} = 0.9375\text{ m (unswept rectangle case)}$$

**Step 3, Combine (moment balance):**

$$x_{CP} = \frac{(2.000)(0.200) + (9.689)(0.9375)}{2.000 + 9.689} \approx 0.811\text{ m}$$

**Step 4, Static margin:**

$$SM = \frac{0.811 - 0.650}{0.100} \approx 1.61\text{ calibers}$$

**What it means:** CP sits behind CG by ~1.6 body diameters, under these simplified assumptions, the design is preliminarily stable and within the typical healthy range.

*(Full derivations, nose slender-body integration, fin aspect-ratio correction, mean-aerodynamic-chord CP location, are in the companion research doc for anyone who wants the math.)*

## 4. Suggestions for the Flight Performance Analyzer

- Guided input form for nose/body/fin/CG measurements
- Auto-calculate per-component $C_{N_\alpha}$ and CP, then total CP
- Side-view diagram plotting CG and CP
- Report SM in both raw length and calibers
- Recompute at launch **and** burnout, since CG shifts as motor mass burns
- Flag when CP is ahead of CG, or when inputs push into unsupported transonic flow
- Use this worked example ($x_{CP}=0.811$ m, $SM=1.61$ cal) as a regression test case

## Reference

James S. Barrowman, *The Practical Calculation of the Aerodynamic Characteristics of Slender Finned Vehicles*, NASA, 1967. [NASA Technical Reports Server](https://ntrs.nasa.gov/citations/20010047838)
