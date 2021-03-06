# Viscosometry Drop
An experimenting suite for ball drop viscosometry experiments.

Created by William Davis.

I wrote and used this code in 2020 for the class EPS 290 Computational Fluid Dynamics, taught by Professor Bruce Buffett at UC Berkeley. 

# Motivation

In various geophysical studies we are interested in the viscosities of various fluids, and ideally we would like to experimentally measure these viscosities. Examples include silicate melts in igneous settings (e.g. Reid et al., 2003), as well as iron alloys at core pressure and temperature conditions (e.g. Dobson et al., 2000). One such experimental technique to do this is a "falling ball viscosometry experiment". A ball/sphere falling through a fluid will reach a terminal velocity that is dependent on the viscosity of the fluid, the radius of the ball, gravitational acceleration, and the density contrast between the ball and the fluid. This is a result of [Stokes' law](https://en.wikipedia.org/wiki/Stokes%27_law). If the other parameters can be measured, then the viscosity can be determined. 

For geophysical fluids, it is often the case that the experiments must be performed at very high pressures and temperatures. As a result, the meduim that the ball falls through must be made very small. Commonly, experiments are conducted in a multi-anvil press. The ball position and terminal velocity is measured by shining brilliant x-rays through the sample as the experiment is progressing.

<img src="https://user-images.githubusercontent.com/38541020/103841545-b3d6d380-5048-11eb-9ebf-d31c76fdb8d3.png" height="400"/>

Clockwise from top-left: sample capsule containing material that will be melted and ball that will fall; octahedral pressure medium for holding capsule; anvils to apply pressure to sample; multi anvil press. Source: own work (CC BY-SA 3.0 US).

To account for the walls and ends of the capsule, correction terms can be added to Stokes' law, (Faxén, 1925; Maude, 1961). One approporiate question to ask is if these correction terms are neccesary and/or valid (e.g. Kahle et al., 2003). To answer this question I developed this code to conduct a numerical equivalent experiment.

## Numerical setup

This code solves the Navier-Stokes equations with the Boussinesq approximation for buoyancy. The fluid dynamic calculations are performed by [`Oceananigans.jl`](https://github.com/CliMA/Oceananigans.jl), which use a finite volume discretization. A third order Runge-Kutte scheme is used for time-stepping, and a WENO 5 scheme is used for advection terms.

The capsule is modelled as a 2D domain with no-slip boundary conditions, and the ball is defined as a circle of negative buoyancy. 

## Example experiment

The following animations are an experiment performed at ![equation](https://latex.codecogs.com/gif.latex?\mu=10^{-2}\text{&space;Pa&space;s}).

<img src="https://user-images.githubusercontent.com/38541020/103839702-dbc43800-5044-11eb-9057-f98ed97dafa1.gif" height="250"/><img src="https://user-images.githubusercontent.com/38541020/103839734-ef6f9e80-5044-11eb-8c94-792ad088cf04.gif" height="250"/>

## Post-processing

The buoyancy field is converted to bitmap images, and a circle is fitted to the "shadow" at each time-step. Through this, the position and velocity of the falling ball can be tracked through the experiment. The terminal velocity is extracted by determining the maximum velocity during the experiment. Uncertainties in velocity reflect measurement uncertainties in the physical experiment.

<img src="https://user-images.githubusercontent.com/38541020/103839653-bc2d0f80-5044-11eb-91a6-f980a19fef29.png" height="400"/>

# Results

I conducted a series of viscosometry experiments at a fixed viscosity of ![equation](https://latex.codecogs.com/gif.latex?\mu=10^{-2}\text{&space;Pa&space;s}) and varied the radius of the sphere dropped. The terminal velocity varies as follows.

<img src="https://user-images.githubusercontent.com/38541020/103836041-ba5f4e00-503c-11eb-920a-e3c43dd219f7.png" height="400"/>

Additionally, I fixed the ball radius and varied the viscosity.

<img src="https://user-images.githubusercontent.com/38541020/103837076-cdbfe880-503f-11eb-9d71-3b786323a58a.png" height="400"/>

# Conculsions

Wall and edge effects are necessary and valid for experiments with large spheres. This effect seems to be larger at lower viscosities, which is slightly counterintuative. This work shows that, when conducting the true physical version of this experiment, workers can get more accurate and precise results by using spheres that are as small as possible. 

# Using this code

I invite people to use this code to continue this work, or use it for similar research. Examples of how to use the code can be found in the `examples/` directory, in script and notebook format. Additionally, a Google colab notebook running an example experiment can be found [here](https://colab.research.google.com/drive/1C23KVG2CVPfwW_wVtFZBAbukL0TNXLTK?usp=sharing).

# Future work

- [ ] Expand to 3D simulations.
- [ ] Explore fluids of different viscosities.
  - [ ] E.g. silicate melts.
- [ ] Explicitally model the time dependence of temperature in the experiment.

# References

Dobson, David P., et al. "In situ measurement of viscosity of liquids in the Fe-FeS system at high pressures and temperatures." American Mineralogist 85.11-12 (2000): 1838-1842.

Faxén, Hilding. Gegenseitige Einwirkung zweier Kugeln, die in einer zähen Flüssigkeit fallen. R. Friedländer & Sohn, 1925.

Kahle, Andreas, Björn Winkler, and Bernard Hennion. "Is Faxén’s correction function applicable to viscosity measurements of silicate melts with the falling sphere method?." Journal of non-newtonian fluid mechanics 112.2-3 (2003): 203-215.

Maude, A. D. "End effects in a falling-sphere viscometer." British Journal of Applied Physics 12.6 (1961): 293.

Reid, Joy E., et al. "The viscosity of CaMgSi2O6 liquid at pressures up to 13 GPa." Physics of the Earth and Planetary Interiors 139.1-2 (2003): 45-54.

