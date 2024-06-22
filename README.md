
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



```sudo
    for all i,j:
     v_i_j = v_i_j + \Delta t \cdot g 
```
where $g$=gravitational constant and $\Delta t$ is the timestep.

#### Making the Fluid Incompressible (projection)
To make fluid incompressible, we need to introduce the idea of divergence (i.e., total outflow). Divergence is proportional to the amount of fluid that leaves the cell in a timestep. 

The simulator sums up each vector in the cell. But it needs to be careful for three cases:
- Divergence is positive: too much outflow
- Divergence is negative: too much inflow 
- Divergence is zero: incompressible (desired!)

Note that if all velocity vectors are directed toward the center of the cell, there will be too much inflow.


<p align="center">
<img src="./content/image1.png" alt="Image 1" style="width:20%; border:0;">
</p>

Even if we change one velocity vector, this is not an ideal representation of a fluid since it cannot do that in real life. Therefore, for fluids, all velocities must be pushed by the same amount. Therefore, to make divergence equal to 0, we set:

$$d = u_{i+1, j} - u_{i, j} + v_{i,j+1} - v_{i,j}$$

$$u_{i+1,j}=u_{i+1,j}-\frac{d}{3}$$

$$v_{i,j}=v_{i,j}+\frac{d}{3}$$

$$v_{i,j+1}=v_{i,j+1}-\frac{d}{3}$$

Note for a wall, $u_{i,j}$ is 0. For moving object, or turbine pushing fluid (i.e., wind tunnel), $u_{i,j}$ is not zero.

For a more general case, the simulation stores a scalar value $s$ in each cell, where $s=0$ for walls and obstacles, and $s=1$ for fluid cells. 

$$s=s_{i+1,j} + s_{i-1,j}+s_{i,j+1} + s_{i,j-1}$$

$$u_{i+1,j}=u_{i+1,j}-d\frac{s_{i+1,j}}{s}$$

$$ u_{i,j}=u_{i,j}+d\frac{s_{i-1,j}}{s}$$

$$v_{i,j}=v_{i,j}+d\frac{s_{i,j-1}}{s}$$

$$v_{i,j+1}=v_{i,j+1}-d\frac{s_{i,j+1}}{s}$$

To run across all cells in grid, we loop through each cell. This method is called *Gauss-Seidel Method*. But we must also consider that on the boundary of the grid, we access cells that are outside of the grid. To fix this, the simulator is set to:
- Add border cells
- Set $s_{i,j}=0$ for walls
- Copy neighbouring values that are inside the grid

