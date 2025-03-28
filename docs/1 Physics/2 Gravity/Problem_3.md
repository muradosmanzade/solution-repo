# Problem 3

Paths of a Freely Dropped Object Near Earth

## 1. Theoretical Foundation

### Categories of Potential Paths
The motion of an object released near Earth is determined by its starting velocity \( v \) in relation to Earth's gravitational attraction. The potential paths include:


1. **Suborbital (Ballistic Path)**: If the object's speed is below orbital velocity, it follows a curved path before falling back to Earth.  
2. **Orbital (Elliptical Path)**: If the speed is between the first cosmic velocity \( v_1 \) (minimum orbital speed) and escape velocity \( v_2 \), the object moves in a stable elliptical orbit.  
3. **Escape (Hyperbolic Path)**: If the speed surpasses escape velocity \( v_2 \), the object follows a hyperbolic course and permanently leaves Earth's gravitational influence.  

These motions adhere to Newton’s Law of Universal Gravitation:  
$$
F = \frac{GMm}{r^2}
$$  
and are further described by Kepler’s Laws of Planetary Motion.

---

## 2. Mathematical Analysis  

### Equations of Motion  
The trajectory of the payload follows Newton’s Second Law:  
$$
\frac{d^2\mathbf{r}}{dt^2} = -\frac{GM}{r^3} \mathbf{r}
$$  

where:  
- \( \mathbf{r} \) represents the position vector of the payload,  
- \( G \) is the universal gravitational constant,  
- \( M \) is Earth’s mass.  

To determine the trajectory accurately, numerical methods like the Runge-Kutta integration technique are commonly employed.

---

## 3. Computational Model
The following Python script simulates and visualizes the trajectory of a payload released near Earth.

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# Constants
G = 6.67430e-11  # Gravitational constant (m^3/kg/s^2)
M = 5.972e24     # Earth mass (kg)
R = 6.371e6      # Earth radius (m)

# Equations of motion
def equations(t, y):
    x, vx, y, vy = y
    r = np.sqrt(x**2 + y**2)
    ax = -G * M * x / r**3
    ay = -G * M * y / r**3
    return [vx, ax, vy, ay]

# Initial conditions (position in km, velocity in km/s)
x0, y0 = R + 500000, 0  # Initial altitude: 500 km
vx0, vy0 = 7.8e3, 0  # Initial velocity (near orbital velocity)
y_init = [x0, vx0, y0, vy0]

# Time span
t_span = (0, 10000)
t_eval = np.linspace(0, 10000, 1000)

# Solve ODE
sol = solve_ivp(equations, t_span, y_init, t_eval=t_eval, method='RK45')

# Plot trajectory
plt.figure(figsize=(6,6))
plt.plot(sol.y[0], sol.y[2], label='Payload Trajectory')
plt.scatter(0, 0, color='blue', label='Earth')
plt.xlabel('X Position (m)')
plt.ylabel('Y Position (m)')
plt.title('Trajectory of a Freely Released Payload')
plt.legend()
plt.grid()
plt.show()
```

This script:
- Defines gravitational equations of motion.
- Uses numerical integration to compute the trajectory.
- Plots the resulting trajectory.

![Trajectory of a Freely Released Payload](images/problem%203.PNG   )

---

## 4. Practical Applications

- **Satellite Placement**: Determining the right velocity for a stable orbit.  
- **Atmospheric Reentry**: Computing angles and speeds for controlled descent.  
- **Deep Space Missions**: Planning trajectories for interplanetary travel.  

---

## 5. Conclusion  

Analyzing the motion of a released payload is essential for space operations. By evaluating velocity and gravitational effects, we can predict whether an object will fall back to Earth, stay in orbit, or escape into space.  
