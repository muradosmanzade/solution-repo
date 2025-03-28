# Problem 1

Orbital Period and Orbital Radius

## 1. Theoretical Basis

### Kepler’s Third Law
Kepler’s Third Law states that the square of a planet’s orbital period \( T \) is proportional to the cube of its orbital radius \( R \):

$$
T^2 \propto R^3
$$
For a circular orbit, this law can be derived using Newton’s law of gravitation and centripetal force:

1. **Gravitational Force as Centripetal Force:**  
   $$  
   \frac{GMm}{R^2} = m \frac{v^2}{R}  
   $$  

   where:  
   - \( G \) is the universal gravitational constant,  
   - \( M \) is the mass of the central body,  
   - \( m \) is the mass of the orbiting body,  
   - \( R \) is the orbital radius,  
   - \( v \) is the orbital velocity.  


2.**Orbital Velocity from Period:**  
   The orbital velocity \( v \) is related to the orbital period \( T \) as:  
   $$  
   v = \frac{2 \pi R}{T}  
   $$  


3. **Deriving Kepler’s Third Law:**
   Substituting \( v \) into the force equation and solving for \( T \), we get:
   $$
   T^2 = \frac{4 \pi^2}{GM} R^3
   $$
   This confirms that \( T^2 \propto R^3 \), where the proportionality constant depends on \( G \) and \( M \).

### Consequences  

**Kepler’s Third Law Derivation:**  
Substituting \( v \) into the force equation and solving for \( T \), we get:  
   $$  
   T^2 = \frac{4 \pi^2}{GM} R^3  
   $$  
This confirms that \( T^2 \propto R^3 \), with the constant of proportionality depending on \( G \) and \( M \).  

**Applications in Astronomy**  
- **Calculating Planetary Mass:** By knowing the period and radius of a planet's moon, we can determine the mass of the planet.  
- **Estimating Distances:** If we know the period of a planet's orbit around the Sun, we can estimate its orbital radius.  
- **Satellite Orbits:** This is used to design stable orbits for satellites around Earth and other celestial bodies.  


---

## 2. Real-World Examples

1. **The Moon's Orbit Around Earth**  
   - The Moon revolves around Earth with a period of \( T = 27.3 \) days.  
   - The average orbital radius is about \( 3.84 \times 10^5 \) km.  
   - By using Kepler's law, we can confirm Earth's mass.

2. **Planets in the Solar System**  
   - Kepler's law helps us compare the orbits of planets.  
   - Example: Earth’s orbital radius of \( 1 \) AU and period of \( 1 \) year help us determine the distances of other planets.

---

## 3. Numerical Simulation  
The following Python script simulates circular orbits and confirms the validity of Kepler's Third Law.  

```python
import numpy as np
import matplotlib.pyplot as plt

# Constants
G = 6.67430e-11  # Gravitational constant (m^3 kg^-1 s^-2)
M_sun = 1.989e30  # Mass of the Sun (kg)

# Function to calculate orbital period
def orbital_period(radius, mass=M_sun):
    return 2 * np.pi * np.sqrt(radius**3 / (G * mass))

# Generate data
radii = np.logspace(9, 12, 100)  # Orbital radii from 10^9 to 10^12 meters
periods = orbital_period(radii)

# Verify Kepler's Third Law
T_squared = periods**2
R_cubed = radii**3

# Plot T^2 vs R^3
plt.figure(figsize=(8,6))
plt.plot(R_cubed, T_squared, label="$T^2 \propto R^3$", color='b')
plt.xlabel("Orbital Radius Cubed (m^3)")
plt.ylabel("Orbital Period Squared (s^2)")
plt.title("Verification of Kepler's Third Law")
plt.legend()
plt.grid()
plt.show()
```
This script:
- Calculates orbital periods for varied radii.
- Visualizes \( P^2 \) against \( r^3 \) to validate the linear correlation.

![Validation of Kepler's Third Law](images/problem%201.PNG)

---

## 4. Expansions and Constraints

- **Elliptical Trajectories:** Kepler’s Law remains valid, with \( r \) denoting the semi-major axis.
- **Relativistic Corrections:** Einstein’s theory of relativity adjusts Kepler’s laws in intense gravitational environments.
- **External Disturbances:** Gravitational interactions from other celestial bodies can perturb orbits over extended periods.

---

## 5. Summary

Kepler’s Third Law succinctly connects orbital period and radius, facilitating calculations in celestial mechanics. This relationship remains indispensable in astronomy, satellite design, and space exploration endeavors.

---

## 4. Extensions and Limitations

- **Elliptical Orbits:** Kepler’s Law still applies, but in this case, \( R \) represents the semi-major axis of the ellipse.
- **Relativistic Effects:** In strong gravitational fields, general relativity adjusts Kepler’s laws, especially for objects moving at high speeds.
- **External Forces:** The gravitational influence of other celestial bodies can cause orbital changes over time, resulting in perturbations.


---

## 5. Conclusion  
Kepler's Third Law provides a clear connection between orbital period and radius, allowing for precise calculations in the field of celestial mechanics. This principle remains essential in astronomy, satellite design, and space exploration.
