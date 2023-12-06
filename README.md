# Tesla Valve CFD Simulations

## Introduction

A Tesla valve also called a valvular conduit by its inventor, Nikola Tesla, is a fixed-geometry passive check valve. It allows fluid to flow preferentially in one direction, without moving parts. The interior of the conduit is provided with enlargements, recesses, projections, baffles, or buckets which, while offering virtually no resistance to the passage of the fluid in one direction, other than surface friction, constitute an almost impassable barrier to its flow in the opposite direction. Tesla illustrated this with the drawing, showing one possible construction with a series of eleven flow-control segments, although any other number of such segments could be used as desired to increase or decrease the flow regulation effect. With no moving parts, Tesla valves are much more resistant to wear and fatigue, especially in applications with frequent pressure reversal such as a pulsejet.
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/TeslaValve.png" width="650"/> 
</div>

The Tesla valve is used in microfluidic applications and offers advantages such as scalability, durability, and ease of fabrication in a variety of materials. It is also used in macrofluidic applications.

## Governing Equations

We shall assume a viscous, incompressible fluid flow for which the Navier-Stokes equation is utilized, given by the continuity and momentum equations as shown below: \
**Continuity Equation:**
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/continuity%20equation.png" width="100"/> 
</div>

**Momentum Equation:**
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/momentum%20equation.png" width="500"/> 
</div>
where $\rho$ is the density of the fluid, $\vec{V}$ is the fluid velocity vector, $p$ is the fluid pressure, $\vec{g}$ is the acceleration due to gravity, $\mu$ is the dynamic viscosity of the fluid and $\nabla^{2}$ is the Laplacian operator.

In the simulations, Navier-Stokes equations are solved using the PISO algorithm, an algorithm that consists of iterative procedures for coupling equations for momentum and mass conservation. PISO algorithm solves a pressure equation, to enforce mass conservation, with an explicit correction to velocity to satisfy momentum conservation. In the pisoFoam solver, kinematic pressure $(m^{2}/s^{2})$ and velocity $(m/s)$ are considered to be mandatory input requirements.

Large Eddy Simulation (LES) turbulence modeling is utilized for the simulations. In LES, the mesh is required to resolve the viscous sub-layer and requires high order schemes to adequately resolve the high-energy
containing eddies. The turbulence kinetic energy is given by:
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/turbulentKE.png" width="500"/> 
</div>

