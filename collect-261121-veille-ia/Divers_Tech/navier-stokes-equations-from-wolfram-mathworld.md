---
id: collect-261121-veille-ia/divers-tech/navier-stokes-equations-from-wolfram-mathworld
title: "Navier-Stokes Equations -- from Wolfram MathWorld"
domain: divers-tech
role: reference
task: article
actors: []
dates: ["2026-09-08"]
keywords: ["equations", "navier-stokes", "velocity", "smooth", "force", "fluid", "equation", "problem"]
source: http://mathworld.wolfram.com/Navier-StokesEquations.html
source_anchor: ""
source_lines: [1, 39]
section: "Divers_Tech"
---
The Navier-Stokes equations are nonlinear partial differential equations governing the velocity and pressure of a viscous fluid. For an incompressible Newtonian fluid with constant density, they can be written

|  |  |  |  (1)  | 
|  |  |  |  (2)  | 

where is the fluid velocity, is the pressure, is the constant fluid density, is the kinematic viscosity, and is the body force per unit mass. The first equation expresses conservation of momentum, while the second is the incompressibility condition.

The nonlinear term is the convective part of the convective derivative. Setting and gives Euler's equations of inviscid motion. Taking the divergence of the momentum equation and using incompressibility gives Poisson's equation

|  |  (3)  | 

so the pressure is coupled to the velocity field. A particular flow problem also requires initial conditions and boundary conditions.

Leray (1934) established the global existence of weak solutions on for divergence-free initial velocity fields having finite norm. His framework allows possible singularities and does not establish uniqueness. It has remained unknown whether every smooth divergence-free initial velocity with suitable decay gives a globally smooth solution. This existence and smoothness question is one of the Millennium Prize Problems (Fefferman 2000).

Tao (2016) constructed a smooth solution that develops finite-time blow-up for an averaged three-dimensional Navier-Stokes equation having the same energy identity and scaling as the classical equations. Since the averaging changes the convective term, this is not a singularity of the classical Navier-Stokes equations. It instead shows that a regularity proof must use finer structure in the nonlinear term than the energy identity and scaling alone.

Córdoba and Martínez-Zoroa (2023) developed an analytic, non-self-similar cascade mechanism in which infinitely many localized regions of vorticity
 interact across successively smaller scales. They used it to produce blow-up for
 the forced three-dimensional Euler equations, but their
 force had only 
 regularity. In a related unforced construction, Córdoba *et al.* (2025)
 obtained a finite-time singularity with velocity
 globally in 
 and smooth away from one point. These regularity limitations
 meant that neither result settled the classical smooth-data problem, but the cascade
 mechanism supplied the main analytic strategy for the later Euler and Navier-Stokes
 constructions (Kakaes 2026).

OpenAI (2026dh) announced a proposed resolution on Sep. 8, 2026. For every positive viscosity, the paper constructs a smooth external force having compact support in space and time and a solution starting from rest that is smooth for . Its norm, and hence its kinetic energy, remains uniformly bounded, but its velocity becomes unbounded as approaches 1. The paper claims that this establishes breakdown alternatives (C) and (D) in Fefferman's official formulation, respectively on and the three-dimensional torus .

Alternatives (C) and (D) explicitly permit a nonzero smooth external force, whereas alternatives (A) and (B) set the force to zero. Therefore, if the proof is correct, it resolves the official problem, but does not establish finite-time singularity formation for the unforced Navier-Stokes equations. OpenAI also released a Lean 4 formalization reporting no omitted proof placeholders. Its project metadata described the review status on release as "self-assessed" (OpenAI 2026g).

The Clay Mathematics Institute (2026) responded on Sep. 11, 2026, describing the problem as "apparently" settled while emphasizing that its process for evaluating the achievement and assigning credit would proceed under the prize rules. This announcement acknowledges the claimed resolution without announcing a prize award.

The animation above (Phadikar 2026) illustrates a concentrating vortex core in scaled coordinates using approximate equations.

The Navier-Stokes equations appear in Big Weld's office in the 2005 animated film
*Robots*.
