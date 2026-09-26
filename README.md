# Descriptor-system-observer
# Descriptor System State Observer — MATLAB/Simulink

A MATLAB/Simulink implementation of **state estimation for a linear descriptor (singular) system**.

The project demonstrates how an observer can estimate internal system states that are not directly available from measurements, using the system inputs and measured outputs.

---

## Overview

In many dynamic systems, it is not possible to directly measure every internal state.

For example, a system may have several internal variables describing its dynamics, while sensors provide only a limited number of outputs.

A **state observer** provides a mathematical way to estimate these unmeasured states from:

* Known system inputs
* Available system outputs
* A mathematical model of the system

In this project, the system is represented in **descriptor form**:

$$
E\dot{x}=Ax+Bu
$$

$$
y=Cx
$$

where \(E\) is a **singular matrix**. Therefore, the system is different from a conventional state-space model where \(E=I\).

---

## What Type of Observer?

The project implements an **observer for a linear descriptor system**.

The observer design uses:

* **Coordinate transformation** using matrices \(R\) and \(V\)
* **Pole-placement based observer gain design**
* **Lyapunov-based analysis**
* **Generalized inverse** for gain calculation

The observer gain \(K\) is obtained using pole placement with desired observer poles:

$$
[-2.34,\;-2.45,\;-3.4,\;-3.3]
$$

The resulting observer dynamics are then analyzed through their eigenvalues.

---

## What Does the Observer Actually Do?

The basic idea is:

```text
             ┌─────────────────┐
   Inputs ──►│                 │
             │  Actual System  │───► Measured Outputs
             │                 │
             └─────────────────┘
                       │
                       │
                       ▼
                ┌──────────────┐
                │   Observer   │
                └──────────────┘
                       │
                       ▼
                Estimated States
```

The actual system has internal states:

$$
x=[x_1,x_2,x_3,x_4]^T
$$

but these states are not necessarily all directly measurable.

The observer uses the available information to reconstruct an estimate:

$$
\hat{x}=[\hat{x}_1,\hat{x}_2,\hat{x}_3,\hat{x}_4]^T
$$

The objective is for the estimated states to converge toward the actual system states.

In simple terms:

> **The observer acts like a mathematical sensor for states that cannot be measured directly.**

---

## System Model

The main MATLAB implementation defines a descriptor system using the matrices \(E\), \(A\), \(B\), and \(C\).

The matrix \(E\) is singular, making this a descriptor/singular system rather than a standard state-space representation.

The model contains:

* 4 system states
* 3 system inputs
* 3 measured outputs

The main system matrices are defined in `fullyobserv.m`.

---

## Observer Gain Design

The observer gain is calculated using MATLAB's pole-placement method.

The desired observer poles are selected as:

```matlab
[-2.34 -2.45 -3.4 -3.3]
```

The gain is then obtained from the transformed system using:

```matlab
K = place((R*A)',(-C)',[-2.34 -2.45 -3.4 -3.3]')';
```

The resulting observer dynamics are represented through:

```matlab
N = R*A + K*C;
```

and the eigenvalues of the resulting matrix are checked to evaluate the observer dynamics.

---

## Lyapunov Analysis

A Lyapunov equation is also used as part of the observer design:

```matlab
Q = eye(4);
P = lyap(N,Q);
```

The resulting matrix \(P\) is then used to calculate:

```matlab
lambda = inv(P)*C';
```

This provides additional information for the stability-related analysis and subsequent gain calculations.

---

## Generalized Inverse

The project also contains a custom MATLAB function:

```text
ginv.m
```

This function calculates a **generalized inverse** of a matrix.

It is used in the main script for:

```matlab
gain1 = ginv(C*R*eta_a)*(C*lambda);
```

A generalized inverse is useful when a conventional matrix inverse cannot be directly applied.

The implementation checks the matrix rank and constructs a generalized inverse satisfying:

$$
XGX=X
$$

---

## Project Files

```text
descriptor-system-observer/
│
├── README.md
│
├── matlab/
│   ├── fullyobserv.m
│   └── ginv.m
│
├── simulink/
│   └── try3.slx
│
└── results/
    └── figures/
```

### `fullyobserv.m`

Main MATLAB script containing:

* Descriptor system definition
* Coordinate transformation
* Observer gain calculation
* Pole placement
* Eigenvalue analysis
* Lyapunov calculation
* Generalized-inverse based gain calculation

### `ginv.m`

Custom generalized-inverse function used by the main script.

### `try3.slx`

Simulink model associated with the observer/system simulation.

---

## Technologies

* MATLAB
* Simulink
* Control Systems
* State Estimation
* Descriptor Systems
* Observer Design
* Pole Placement
* Lyapunov Stability Analysis
* Matrix Algebra

---

## Key Concepts

| Concept             | Purpose                                                                    |
| ------------------- | -------------------------------------------------------------------------- |
| Descriptor system   | Represents systems using \(E\dot{x}=Ax+Bu\), including singular \(E\)      |
| State observer      | Estimates internal states that cannot be directly measured                 |
| Pole placement      | Controls the desired observer dynamics                                     |
| Lyapunov analysis   | Used to analyze stability properties                                       |
| Generalized inverse | Provides an inverse-like solution for non-standard/rank-deficient matrices |
| MATLAB/Simulink     | Used for implementation and simulation                                     |

---

## Status

This repository is a **reconstruction of an earlier academic MATLAB/Simulink project** using the original source files that were recovered.

The implementation will be further documented with simulation results and plots as the original model is reconstructed and validated.

---

## Learning Outcome

This project demonstrates practical experience with:

* Mathematical modeling of dynamic systems
* Descriptor/singular state-space systems
* State estimation
* Observer gain design
* Pole placement
* Stability analysis
* MATLAB programming
* Simulink-based system simulation
