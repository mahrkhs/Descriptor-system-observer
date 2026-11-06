# Descriptor System State Observer

MATLAB/Simulink implementation of a state observer for linear descriptor systems.

The project explores state estimation in systems represented by

$$
E\dot{x}=Ax+Bu,\qquad y=Cx
$$

where \(E\) is a singular matrix.

Unlike conventional state-space models, descriptor systems can represent both dynamic and algebraic relationships. The objective here is to estimate the system states using the available inputs and measured outputs.

## Implementation

The MATLAB implementation includes:

- Definition of a four-state descriptor system with three inputs and three outputs.
- Coordinate transformation using matrices \(R\) and \(V\).
- Observer gain calculation using pole placement.
- Eigenvalue analysis of the resulting observer dynamics.
- Lyapunov equation for stability-related analysis.
- Generalized inverse calculation for the gain computation.


A custom MATLAB function (`ginv.m`) is included for the generalized inverse calculation.

## Project Structure

```text
descriptor-system-observer/
│
├── matlab/
│   ├── fullyobserv.m
│   └── ginv.m
│
├── simulink/
│   └── try3.slx
│
├── results/
│   └── figures/
│
└── README.md
```



