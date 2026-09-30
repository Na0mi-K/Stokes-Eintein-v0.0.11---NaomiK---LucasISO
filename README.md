# Stokes-Eintein-v0.0.11---NaomiK---LucasISO

***This program takes the temperature, viscosity, size, and shape of a microscopic particle, uses the Stokes–Einstein equation to calculate its diffusion coefficient, generates a physically motivated random 3D Brownian trajectory, validates that trajectory against theoretical Brownian-motion statistics, and optionally turns the result into a 3D GIF animation.
just saying : The simulation is a model, not a full molecular-dynamics simulation. It doesn't simulate individual solvent molecules colliding with the particle. Instead, it jumps directly to the statistical consequence of those collisions: Gaussian random displacements whose variance is determined by \(D\). That is why it's computationally lightweight while still reproducing the expected Brownian-motion statistics.***

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

A Python application that simulates the 3D Brownian motion of a microscopic particle using the Stokes–Einstein equation.

The program lets you specify physical conditions such as temperature, viscosity, particle size, and geometry. It then calculates the particle's diffusion coefficient and uses that value to generate a random 3D trajectory.

It also provides several diagnostics to compare the simulation against theoretical predictions and can generate an animated GIF of the particle's motion.

# What does it simulate?

The program models a particle suspended in a fluid. At microscopic scales, particles undergo Brownian motion: instead of moving in a straight line, they are constantly pushed around randomly by collisions with surrounding molecules. The simulation represents this as a sequence of random movements . The size of those random movements is determined by the particle's diffusion coefficient D : 

<img width="242" height="236" alt="Image" src="https://github.com/user-attachments/assets/6184e092-1e7d-4850-b216-3f0bcdafb951" />

 The central equation is the Stokes–Einstein relation : 
      $$D = \frac{k_B T}{6 \pi \eta r}$$ 

<img width="273" height="142" alt="Image" src="https://github.com/user-attachments/assets/141adc81-ba7e-404a-b9d8-138c7a004524" />

# Intuitively

The equation says:
- Higher temperature → more diffusion
- Higher viscosity → less diffusion
- Larger particle → less diffusion

So, for example, a small particle in hot water will generally move around more rapidly than a large particle in a cold, viscous liquid (shame on people who dont know that)

**The GUI allows two shapes:**
Ball *(ball cancer)*
Cube *(The CUBE)*

For a sphere, the hydrodynamic radius is simply:

$$ r = \frac{\text{size}}{2} $$

For the cube, the program does something slightly different. It calculates the radius of a sphere having the same volume as the cube:

$$ r = \left( \frac{3s^3}{4\pi} \right)^{1/3} $$

where \(s\) is the cube's side length.

We explicitly noted in the GUI: the Stokes–Einstein equation is strictly spherical, so the cube treatment is an approximation. So the cube isn't actually using a cube-specific drag equation because we are lazy as fuck . It's essentially:

```
Cube
  │
  ▼
Calculate its volume
  │
  ▼
Find equivalent-volume sphere
  │
  ▼
Use that sphere's radius
  │
  ▼
Stokes–Einstein equation
```
# How the Brownian motion is generated

Once D has been calculated, the program determines how large each random movement should be. It calculates:

$$ \sigma = \sqrt{2D\Delta t} $$

where ***dt*** is the simulation timestep. it happens here : ``` sigma = math.sqrt(2.0 * D * params.dt) ```
Then it generates random movements in all three dimensions: 
```
increments = rng.normal(
    0.0,
    sigma,
    size=(n_steps, 3)
)
```
In other words, every timestep gets this down below *(The three coordinates are independent Gaussian random variables.)*
Δx   Δy   Δz
 │    │    │
 ▼    ▼    ▼
random random random
movement movement movement

# Building the trajectory

The particle starts at:

$$ (x,y,z)=(0,0,0) $$

The program then accumulates every random displacement. For example, imagine the random movements were:
```
Step 1: (+1, -2, +1)
Step 2: (-2, +1,  0)
Step 3: (+1, +1, -2)
```
The positions become:
```
Start:  ( 0,  0,  0)
Step 1: ( 1, -2,  1)
Step 2: (-1, -1,  1)
Step 3: ( 0,  0, -1)
```
With Numpy : ```positions[1:] = np.cumsum(increments, axis=0)```

# Mean squared displacement (MSD)

**One of the most important things the program checks is the mean squared displacement.**
For 3D Brownian motion, theory predicts:

$$ \langle r^2(t)\rangle = 6Dt $$

The program calculates the theoretical prediction with: ```msd_theory = 6.0 * D * t ```
It also calculates the simulated displacement: $$r^2 = x^2 + y^2 + z^2$$ with :
```
squared_radius = np.sum(positions**2, axis=1)
msd = squared_radius
```
*One subtle point: despite the variable being called msd, this is technically the squared displacement from the initial position for one trajectory, rather than an ensemble/time-averaged MSD. So the graph is a stochastic trajectory compared against the theoretical mean.*

# The diagnostic graphs

*After the simulation, the program creates four plots.*

**1. 3D Brownian trajectory**

The first graph shows the particle's path:
<img width="463" height="299" alt="Image" src="https://github.com/user-attachments/assets/c2f04ea1-409e-4032-a866-e555e8576959" />
























     
