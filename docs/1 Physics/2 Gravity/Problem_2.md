# Problem 2

Escape Velocities and Cosmic Velocities

## 1. Theoretical Foundation

### Defining Cosmic Velocity Thresholds
Cosmic velocities delineate the minimal speeds necessary to achieve distinct modes of motion in space:

1.  **First Cosmic Velocity (Orbital Velocity)**
    -   The minimal velocity required to attain a stable circular trajectory around a celestial body.
    -   Derived from the equilibrium between gravitational attraction and centripetal acceleration:
        $$
        v_1 = \sqrt{\frac{GM}{R}}
        $$

2.  **Second Cosmic Velocity (Escape Velocity)**
    -   The velocity threshold required to overcome a celestial body's gravitational pull without any additional thrust.
    -   Derived from the principle of energy conservation:
        $$
        v_2 = \sqrt{\frac{2GM}{R}}
        $$
    -   It is noteworthy that \( v_2 = \sqrt{2} v_1 \).

3.  **Third Cosmic Velocity (Solar System Ejection Velocity)**
    -   The velocity needed to leave the Sun’s gravitational domain from a planet’s orbital path.
    -   Calculated by combining the escape velocity from the planet and the required velocity to depart the Sun’s gravitational influence:
        $$
        v_3 = \sqrt{v_2^2 + v_{sun}^2}
        $$
    -   where \( v_{sun} \) represents the planet’s orbital velocity around the Sun.

---


## 2. Mathematical Analysis  

- **Factors Affecting Velocities:**  
  - **Mass (\( M \))**: Higher mass increases the required velocity for orbit or escape.  
  - **Radius (\( R \))**: Smaller celestial bodies demand higher velocities due to intensified surface gravity.  

- **Relation between Velocities:**  
  - Escape velocity always surpasses orbital velocity.  
  - Interstellar travel necessitates exceeding the third cosmic velocity.  


---

## 3. Computational Model  
Below is a Python script that computes and visualizes cosmic velocities for Earth, Mars, and Jupiter.  


```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.constants import G

def cosmic_velocities(mass, radius):
    """Calculate first, second, and third cosmic velocities."""
    v1 = np.sqrt(G * mass / radius)
    v2 = np.sqrt(2) * v1
    return v1, v2

# Celestial bodies data (mass in kg, radius in m)
bodies = {
    "Earth": (5.972e24, 6.371e6),
    "Mars": (6.417e23, 3.389e6),
    "Jupiter": (1.898e27, 6.9911e7)
}

velocities = {body: cosmic_velocities(mass, radius) for body, (mass, radius) in bodies.items()}

# Visualization
labels, v1_vals, v2_vals = zip(*[(body, v[0], v[1]) for body, v in velocities.items()])
x = np.arange(len(labels))
width = 0.35

fig, ax = plt.subplots(figsize=(8,5))
ax.bar(x - width/2, v1_vals, width, label='First Cosmic Velocity (km/s)', color='b')
ax.bar(x + width/2, v2_vals, width, label='Second Cosmic Velocity (km/s)', color='r')

ax.set_xticks(x)
ax.set_xticklabels(labels)
ax.set_ylabel('Velocity (m/s)')
ax.set_title('First and Second Cosmic Velocities')
ax.legend()
plt.grid()
plt.show()
```

This script:
- Calculates orbital and escape velocities for different celestial bodies.
- Plots a comparison of these velocities.

![First and Second Cosmic Velocities](images/problem%202.png    )

---

## 4. Importance in Space Exploration  

- **Satellite Launches:** Achieving the first cosmic velocity ensures satellites remain in stable orbit.  
- **Interplanetary Missions:** Surpassing escape velocity allows spacecraft to travel to other planets like Mars.  
- **Interstellar Travel:** The third cosmic velocity enables spacecraft to exit the Solar System, as demonstrated by Voyager 1.  


---

## 5. Conclusion  

Comprehending escape and cosmic velocities is crucial for advancing space exploration. These fundamental speeds determine satellite positioning, deep space missions, and the possibility of interstellar voyages.  
