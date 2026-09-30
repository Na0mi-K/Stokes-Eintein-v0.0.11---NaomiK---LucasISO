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

                                                         
   ---------------------------------------------------------------------------------------------------------------------------------------------------------------                                                                                                             The central equation is the Stokes–Einstein relation :      $$D = \frac{k_B T}{6 \pi \eta r}$$
          
     
