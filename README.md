# OpenFOAM CFD Simulations of Flow Through a Tesla Valve

## Introduction

A Tesla valve also called a valvular conduit by its inventor, Nikola Tesla, is a fixed-geometry passive check valve. It allows fluid to flow preferentially in one direction, without moving parts. The interior of the conduit is provided with enlargements, recesses, projections, baffles, or buckets which, while offering virtually no resistance to the passage of the fluid in one direction, other than surface friction, constitute an almost impassable barrier to its flow in the opposite direction. Tesla illustrated this with the drawing, showing one possible construction with a series of eleven flow-control segments, although any other number of such segments could be used as desired to increase or decrease the flow regulation effect. With no moving parts, Tesla valves are much more resistant to wear and fatigue, especially in applications with frequent pressure reversals such as a pulsejet.
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

In the simulations, Navier-Stokes equations are solved using the PISO algorithm, an algorithm that consists of iterative procedures for coupling equations for momentum and mass conservation. The PISO algorithm solves a pressure equation, to enforce mass conservation, with an explicit correction to velocity to satisfy momentum conservation. In the pisoFoam solver, kinematic pressure $(m^{2}/s^{2})$ and velocity $(m/s)$ are considered to be mandatory input requirements.

Large Eddy Simulation (LES) turbulence modeling is utilized for the simulations. In LES, the mesh is required to resolve the viscous sub-layer and requires high-order schemes to adequately resolve the high-energy
containing eddies. The turbulence kinetic energy is given by:
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/turbulentKE.png" width="500"/> 
</div>
where the $C_{e}$ and $C_{k}$ coefficients are derived from local flow properties. The cell gradient is calculated using the Gauss Theorem with a cell-based linear interpolation scheme:
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/gausstheorem.png" width="250"/> 
</div>
A backward time scheme is utilized (shown below), which is an implicit, second-order, transient, and conditionally stable scheme. However, boundness is not guaranteed in this scheme.
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/delphi.png" width="300"/> 
</div>
The divergence of a property $Q$ describes the net rate at which it changes as a function of space, represented using the notation:
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/qdivergence.png" width="300"/> 
</div>

Divergence schemes utilized for the project are:  

<ol>
 <li><strong>Limited linear divergence scheme</strong>: This scheme limits towards upwind (first-order bounded) in regions of rapidly changing gradient; requires a coefficient, where 1 is the strongest limiting, tending towards linear (second-order unbounded) as the coefficient tends to 0. 1 is the coefficient utilized in this case. Advection of turbulent kinetic energy uses this scheme.</li>
 <li><strong>LUST divergence scheme</strong>: LUST stands for Linear-Upwind Stabilised Transport. It is a fixed blended 75% linear (second-order unbounded)/ 25% linearUpwind (second-order, upwind-biased, unbounded) scheme, that requires discretization of the velocity gradient to be specified. Advection of velocity utilizes this scheme.</li>
</ol>

Taking the Laplacian of a property $\phi$ is represented using the notation:
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/laplacian.png" width="300"/> 
</div>

or as a combination of divergence and gradient operators:
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/divgrad.png" width="100"/> 
</div>

where $\Gamma$ is a diffusion coefficient.

Laplacian scheme used for the project is based on the Green theorem, requiring the linear interpolation scheme with a corrected surface-normal gradient scheme:

<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/correctionscheme.png" width="400"/> 
</div>

where 

<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/alpha.png" width="100"/> 
</div>

The project utilizes a pisoFoam solver, a transient solver for incompressible, turbulent flow, using the PISO (Pressure Implicit with Splitting of Operators) algorithm. The equation solvers, tolerances, and algorithms are controlled from the fvSolution dictionary in the system directory. GAMG solver is used for solving kinematic pressure.
GAMG stands for Geometric agglomerated algebraic multigrid solver. The generalized method of geometric-algebraic multi-grid (GAMG) uses the principle of generating a quick solution on a mesh with a small number of cells; mapping this solution onto a finer mesh; using it as an initial guess to obtain an accurate solution on the fine mesh. GAMG is faster than standard methods when the increase in speed by solving first on coarser meshes outweighs the additional costs of mesh refinement and mapping of field
data. In practice, GAMG starts with the mesh specified by the user and coarsens/refines the mesh in stages. The user is only required to specify an approximate mesh size at the most coarse level in terms of the number of cells. Smoothers are used along with the solvers to reduce the mesh dependency of the number of iterations. Gauss-Siedel and DICGaussSeidel smoothers are used by the GAMG solver in this project. For the rest of the variables such as velocity, turbulent kinetic energy, etc., Gauss-Siedel smoother is utilized.

<strong>PISO (Pressure Implicit with Splitting of Operators) Algorithm:</strong>
<div align="center">
 <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/pisoalgorithm.png" width="400"/> 
</div>

## Boundary Conditions:

Kinematic pressure and velocity boundary conditions are utilized for running the simulations in the fluid domain. Two types of boundary conditions are used for running the simulations in this case, which are:

1. **Neumann Boundary condition:** $$\frac(\partial{\phi})(\partial{n})=0$$\
   where $\phi$ is kinematic pressure/velocity in this case.
2. **Dirichlet Boundary condition:** $$\phi_{f} = \phi_{ref}$$\
   where $\phi_{ref}$ is the value assigned to kinematic pressure/velocity at the boundary inlet/outlet.

Wherever velocity boundary condition is “Dirichlet”, pressure boundary condition should be “Neumann” and vice versa, which is utilized in this case.

For the inlet, a zeroGradient boundary condition is used for the kinematic pressure which applies a zero-gradient condition from the patch internal field onto the patch faces and a known velocity of $1 m/s$ is used for the velocity. In the case of the outlet boundary conditions, the fixedValue=0 boundary condition is used for kinematic pressure in which a fixed value constraint (here $0 m^{2}/s^{2}$) is applied to the outlet, and the zeroGradient boundary condition is applied for velocity.

## Simulation Results

Simulations are run from time = 0 seconds to time = 0.5 seconds, as by 0.5 secs steady state is achieved in the flow through the Tesla valve and computational effort can be minimized.\
The following are the distributions of kinematic pressure and velocity under 2 cases of flow from left to right and right to left:

### Case 1: Flow from Right to Left

a. Kinematic Pressure

|<img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/kinempress1.png" width="500"/> |  <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/kinempress2.png" width="500"/>  |
|:------:|:------:|
|t = 0 seconds|t = 5 seconds|

b. Velocity

|<img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/vel1.png" width="500"/> |  <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/vel2.png" width="500"/>  |
|:------:|:------:|
|t = 0 seconds|t = 5 seconds|

### Case 2: Flow from Left to Right

a. Kinematic Pressure

|<img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/kinempress3.png" width="500"/> |  <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/kinempress4.png" width="500"/>  |
|:------:|:------:|
|t = 0 seconds|t = 5 seconds|

b. Velocity

|<img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/vel3.png" width="500"/> |  <img src="https://github.com/Karthik-Rajgopal/Tesla-Valve-CFD-Simulations/blob/main/Images/vel4.png" width="500"/>  |
|:------:|:------:|
|t = 0 seconds|t = 5 seconds|

### Inference:

In Case 1 (flow from right to left), we can see an unrestricted flow with adequate velocity for the flow to occur. However, it is not the situation in Case 2 (flow from left to right). In Case 2, the fluid, by the time it attains a steady-state has a very minimal velocity for the flow to occur and higher kinematic pressure is observed compared to Case 1 results due to higher restrictions caused by the fluid from the branches meeting with the fluid through the central portion of the valve. Case 1 has a higher velocity than Case 2 due to minimal restrictions caused to the fluid flow and Case 2 has higher kinematic pressure than Case 1 throughout the valve due to higher restrictions to the fluid flow. Thus, it implies that flow is unrestricted from right to left and is restricted from left to right, thereby allowing a unidirectional flow in the Tesla valve without the usage of any moving parts, which is the biggest advantage of using a Tesla valve.
