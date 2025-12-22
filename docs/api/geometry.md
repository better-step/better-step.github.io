# Geometry Module

The Geometry module handles the representation and processing of geometric entities in CAD models.

## Key Components

- **Curve Processing:** Classes for lines, circles, ellipses, B-splines, etc.
- **Surface Processing:** Classes for Planes, Cylinders, Cones, Spheres, Torus, B-spline, Revolution, and Extrusion surfaces.

### Usage Example

```python
import numpy as np

from abs.curve import BSplineCurve
import abs.sampler as sampler

# Example B-spline curve definition (as stored/represented in ABS)
curve_data = {
    "type": "BSplineCurve",
    "closed": False,
    "continuity": 6,
    "degree": 1,
    "interval": [-3.512e-17, 1.000e+0],
    "knots": [-3.512e-17, -3.512e-17, 1.000e+0, 1.000e+0],
    "poles": [
        [4.217e+0, -2.184e+1, 2.540e+1],
        [4.217e+0, -2.184e+1, 5.080e+1],
    ],
    "rational": False,
    "weights": [1.000e+0, 1.000e+0],
    "transform": [[1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0]],
    "periodic": False,
}

# Construct the curve object
curve = BSplineCurve(curve_data)

# Sample points along the curve
n = 2000
uniform_points = sampler.uniform_sample_curve(curve, n)   # (n, 3)
random_points  = sampler.random_sample_curve(curve, n)    # (n, 3)

print("Uniform samples:", uniform_points.shape)
print("Random samples:", random_points.shape)
print("First 5 uniform points:\n", uniform_points[:5])

```