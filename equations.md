# Newtons second law

For a solid body with constant mass

F =ma
When the mass of the body changes with time
F = d(mv)/dt

# Navier-Stokes equation

For a unit volume of fluid parcel

D (mU)/Dt = F
When we divide the above equation by unit volume, we get

D (rho U)/Dt = f
D/Dt = d/dt + Ux d/dx + Uy d/dy + Uz d/dz
d/dt => change due to time
Ux d/dx + Uy d/dy + Uz d/dz => change in space

D/Dt    = d/dt + Ux d/dx + Uy d/dy + Uz d/dz
        = d/dt + U.∇

D(ρU)/Dt    = (d/dt + U.∇) (ρU)     =f
            = d(ρU)/dt + U.∇ (ρU)   =f
            = d(ρU)/dt +∇.(ρUU)     =f

The external forces are

f   =Pressure + shear stress + gravity
    =-∇p + ∇.τ + ρg

Finally the Navier-Stoke equation become

d(ρU)/dt +∇.(ρUU) = -∇p + ∇.τ + ρg

# Transport equations

Fluid transporting the temperature

d(ρCpT)/dt + ∇.(ρCpTU) = ∇.k∇T + S
∇.(ρCpTU) - Conduction term
∇.k∇T - Convection term

Injection of fine solid particles in to a flow stream
d(ρC)/dt + ∇.(ρCU) = ∇.D∇C + Sc
C - Solid particel Concentration
D - Diffusivity of the solid particel

Common form of a transport equation is

d(ρɸ)/dt + ∇.(ρɸU) = ∇.ℾ∇ɸ + Sɸ

# Fourier law

heat flux qw = -k ∇T . n^ (W/m2)

steady state conduction of heat in solids

k d2T/dxi2 = 0



