
# Eulerian Fluid Simulation

Simple Fluid Simulation modelling behaviour, pressure,  and velocity in the following:
- Wind Tunnel
- Hires Tunnel 
- Paint
- Tank 
- Smoke 

with the option to model with streamlines. 

## simulation assumptions
Fluid and gases have similar physical structures, and thus, the same simulation method will be used to model both.

The model also assumes incompressibility. Just as a side note, water is very close to being incompressible (10,000 km/cm^3 -> 3% compression). This also acts as a resonable assumption for the behaviour of gases as well.

Zero viscosity, which allows a good approximation for gas and water.


## method

 The simulation uses the Grid Method as introduced by Euler. We essentially model a fluid as a velocity field on a Grid. 

 Velocity will be represented by a 2D vector:  
 
 ```math
 \textbf{v} = \begin{bmatrix} u \\ v \end{bmatrix}$$
 ```



which is represented in a collcated grid. It should be noted that velocity vectors are stored in the center of each grid.



### Modifying Velocities to model real behaviour (i.e., adding gravity)

To model real behaviour, we need to add gravity. To do so, we do the following:

```math
for all i,j 
```

```math

    v_i_j = v_i_j + \Delta t \cdot g 
```
where $$g$$=gravitational constant and $$\Delta t$$ is the timestep.
