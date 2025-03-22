## Туториалы по программированию в OpenFOAM

Оригинальные туториалы совместимы с версией OpenFOAM 9 (смотри oldREADME.md).
Данные туториалы совместимы с версией OpenFOAM 12 (на данный момент переписаны первые 11 туториалов, 0 - 11).


## Оригинальные пояснения:

## Tutorial 0 - Hello world

Presents a basic OpenFOAM executable which prints a simple, yet important,
message.

---------
## Tutorial 1 - Input and output

Shows how to read information from dictionaries and output it into files.

---------
## Tutorial 2 - Command line arguments

Shows how to pass arguments and options to custom applications.

---------
## Tutorial 3 - Understanding the mesh

Discusses how the OpenFOAM mesh description works and introduces the code
interface used to interact with the grid.

---------
## Tutorial 4 - Basic field operations

Introduces the idea of a field object, reading values from OF-native files
using built-in operators, as well as calculating field values by hand.

---------
## Tutorial 5 - Basic parallel computing

Gives a crash-course introduction to parallel computing with OpenFOAM and
OpenMPI based on the example "solver" developed in Tutorial 4. The way
OpenFOAM handles parallel domain decomposition is described, basic operators
used for communication between parallel nodes are shown, and the basic solver
is upgraded to work in parallel.

---------
## Tutorial 6 - Custom classes

Shows how a new class may be added to expand OpenFOAM functionality, as well
as gives an example implementation of a class derived from and OpenFOAM
object. This is done by extending from the IOdictionary, with the aim of
adding a custom method which lists the contents of the dict file, while keeping
all of the baseline functionality.

---------
## Tutorial 7 - Custom libraries

Shows how an external library may be compiled and added to OpenFOAM. This is
done by moving the key functionality of the "solver" from Tutorials 4 and 5
into an independent library, and then linking that against the rest of the
solver code.

---------
## Tutorial 8 - Custom boundary condition

Shows how a custom boundary condition may be implemented.
It does not introduce a bespoke utility, but instead only implements a
library. This defines an inlet condition that allows a boundary layer
profile to be prescribed at the inlet of a pipe.

The BC is implemented as a class derived from the fixedValue boundary
condition, adding several control parameters allowing the inlet profile
to be customised. Key elements of the code are highlighted with the keyword
NOTE:. Key methods to pay attention to are the two constructors, default
and one constructing the BC from string, and .updateCoeffs().

The test case is a straight pipe, flow through which gets solved with the
basic simpleFoam solver. Key things to note are the definition of the
BC in 0.org/U and the incorporation of a custom library in system/controlDict.
The simulation is 3D RANS on a coarse mesh so it takes a few minutes on
a low-end machine. The effect of the boundary condition may be visualised
by plotting the x-velocity through the pipe and noting the incident boundary
layer profile at the inlet and how it affects the solution.

---------
## Tutorial 9 - Runtime post processing utility

Discusses the implementation of a runtime post-processing utility which
computes the flow rate through a face zone defined in the mesh using the
topoSet utility.

The utility is implemented as a runtime postprocessing object derived from
the built-in fvMeshFunctionObject and logFiles classes. It integrates the normal
velocity through a specified face zone at each required time step and writes the
result to a file, as well as prints in on the screen. The key methods to
pay attention to are 1) the constructor 2) writeFileHeader(), 3) createFileNames(),
and 4) write(), which implements the actual maths behind the functionality.
Key elements of the code are highlighted with the keyword NOTE:. It is
important to note that the utility gets compiled as a library, which then
gets linked to the main solver, following the OpenFOAM runtime utility
convention.

The test case is the same pipe as in Tutorial 8, except it uses a uniform
inflow BC and is not run until full convergence. It is worth to note
the definition of the faceZone of interest in system/topoSet. This may be
visualised by selecting "Include zones" in paraview and applying the "Extract
block" filter. As the simpleFoam solver is run, the output file gets created
by the utility in the postProcessing directory.

---------
## Tutorial 10 - Transport equation

Introduces the concepts behind solving a simple scalar transport equation.

The solver sets up the transport problem by importing a fixed velocity field
from the last time step and solving the transport of a scalar, beta, in the
presence of the velocity, beta being also subject to diffusion characterised
by a fixed proportionality constant, gamma. The solver is conceptually similar
to the built-in scalarTransportFoam, except it solves a steady-state problem.
Key things to note are 1) the syntax behind the scalar transport equation
2) how OpenFOAM translates the syntax into specific operations and associates
them with entries in system/fvSolution and system/fvSchemes dictionaries
3) inclusion of the boundary condition definitions in 0/beta into the equation
4) units of the equations being solved and how OpenFOAM handles them.

The test case is a simple 2D square domain with fixed scalar inlets at the bottom
and the left-hand side. Transport takes place in the presence of a velocity
field convecting away from the beta inlets. Once the case is run, it is best
to visualise the initial conditions in the "beta" field and the solution to the
transport equation saved as the "result" field.

Recommended reading:
- Wikipedia is always a good start:
    https://en.wikipedia.org/wiki/Convection%E2%80%93diffusion_equation
- Very brief description of the physical and mathematical concepts behind
	the scalar transport equation by CFD-online:
    https://www.cfd-online.com/Wiki/Generic_scalar_transport_equation
- Chapters 3, 4 and especially 5 in "Numerical Methods in Heat, Mass,
    and Momentum Transfer" by Murthy, J. Y. 2002:
    https://engineering.purdue.edu/~scalo/menu/teaching/me608/ME608_Notes_Murthy.pdf

![Alt text](OFtutorial10_transportEquation/testCase/2DconvectionDiffusion.png?raw=true "Tutorial 10 - result of 2D convection-diffusion with inlets at left and bottom edges")

---------
## Tutorial 11 - Modifying the mesh

Demonstrates how to use points to generate different cell types, patches,
and export the finished grid to an OpenFOAM case.

Also recommended to view the 'meshPoints.pdf' or Gmsh files to get a better
idea of how the mesh is actually constructed from points.

![Alt text](OFtutorial11_modifyingTheMesh/testCase/cellTypes.png?raw=true "Tutorial 11 - different cell topologies")

---------
## Tutorial 12 - Adding a custom momentum source

Shows a modified version of the actuatorDisk momentum source which does not use
a cellSet in order to mark cells for applying the source. Instead, it identifies
the cells inside of the constructor which allows easier adjustment of the disk
parameters and could be developed further to include a dynamic variant. Main
part of the implementation is located in "customActuationDiskSourceTemplates.C"
and the cell selection algorithm is implemented in the class constructor inside
"customActuationDiskSource.C". Key takeaways from the tutorial are how a fvOption
object is structured and how it may be modified to suit ones needs. It is a bit
more applied than the previous ones but hopefully will be useful to at least
a few people.

![Alt text](OFtutorial12_momentumSource/testCase/Umagnitude.png?raw=true "Tutorial 12 - velocity affected by a momentum source")

---------
## Tutorial 13 - Waves

Contributed by: Ramkumar

Presents a basic solver that solves the wave equation. The main
motivation for this tutorial is to induce a very basic idea on how to solve custom
equations using OpenFOAM C++. The tutorial "OFtutorial10\_transportEquation" gives
an idea on solving a transport equation on an already solved velocity field. Hence
the equation is solved only once. Whereas in here, the wave equation is solved in
transient form. This solver needs a new field named "h", which is the amplitude of
the wave in the domain.

The test case is a simple 2D wave simulation with initial condition where a single
peak amplitude region was placed at center of 2d plane. The simulation computes
how the waves propagate and induce new waves through reflection, their constructive
and destructive interference also can be clearly seen. Geometry/mesh is a 1X1X0.01
box with single cell thick, and with "empty" BC on top and bottom faces. The video
file "output.mp4" is the simulation output generated as an animation.

![Alt text](OFtutorial13_waveEquationSolver/testCase/waveElevation.png?raw=true "Tutorial 13 - wave elevation")

---------
## Tutorial 14 - The SIMPLE Algorithm

Contributed by: Ramkumar

Shows how the matrix operations are performed in order to solve
the governing flow equations using the SIMPLE algorithm.
It is worth noting that the code is intended to show only how the equations are solved.
Hence, the optimization and convergence check conditions are omitted to make the
code simpler to understand.

The test case used solves a 2D channel flow. You can visualise the results
using the existing Paraview state file by running: ```paraview --state=viewData.pvsm```.

The following are prerequisites that are required for understanding this
tutorial:
- CFD Online wiki entry (very brief and little detail, but succinct):
    https://www.cfd-online.com/Wiki/SIMPLE_algorithm
- This Youtube video uses the same matrix notation as what is in the code:
    https://www.youtube.com/watch?v=ahdW5TKacok
- A rather detailed yet concise notes:
    https://quickersim.com/tutorial/tutorial-2-numerics-simple-scheme

![Alt text](OFtutorial14_SIMPLE_algorithm/testCase/velocity_field.png?raw=true "Tutorial 14 - channel flow velocity distribution")

---------
## Tutorial 15 - Discretisation schemes

Illustrates how the fundamental concept of discretisation is handled inside OpenFOAM
on the example of a convective flux used in the generic scalar transport equation.
The derived scheme merges linear and upwind interpolation in order to demonstrate
how custom behaviour may be implemented. Key place to pay attention to is the
"weights" routine defined in "OFtutorial15.H".

Test case used is a simple 1D flow with a dimensionless scalar field convecting
a jump in the x-direction. Observe how the blended scheme is more accurate than
pure upwind but avoids overshoot of the simple linear scheme.

Recommended reading:
- Excellent slides by WolfDynamics about theory of CFD as used in OpenFOAM:
    http://www.wolfdynamics.com/training/OF_WS2020/traning_session2020.pdf

![Alt text](OFtutorial15_discretisation/testCase/tutorial15.png?raw=true "Tutorial 15 - discretisation")
![Alt text](OFtutorial15_discretisation/testCase/cellCentreValues.png?raw=true "Tutorial 15 - discretisation")

---------
## Tutorial 16 - Lagrangian Particle Tracking

An custom application for tracking a massless particle through an existing flow field
is developed. This tutorial introduces following concepts:

* setting simulation time to read data at particular saved timestep.
* datatypes *point* and *pointList*, which are nothing but vector datatypes dedicated to coordinates.
* a mesh function to get the id of a cell within which a given point coordinates resides.
* Lagrangian massless particle tracking.
* writing VTK file for visualizing the particle's track.

![Alt text](OFtutorial16_particleTracking/testCase/particlePath.png?raw=true "Tutorial 16 - particle tracking")
---------

## Разбор оригинальных туториалов на китайском (до 5-го туториала):
https://www.binyang.fun/openfoam-%e7%bc%96%e7%a8%8b%e5%9f%ba%e7%a1%8001-hello-world-in-openfoam/
