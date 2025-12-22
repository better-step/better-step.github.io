# Sampling Module

Provides methods to sample CAD models for feature extraction.

### Sampling Geometry

Use `sample_parts` to draw samples from one or more CAD parts. In addition to the sampled points `P`, the function returns `S`, which contains per-sample auxiliary data. The behavior is controlled by `lambda_func`: an arbitrary user-defined function that is evaluated during sampling to compute whatever additional attributes you want (e.g., surface normals, curvature features, IDs, etc.).

```python
from abs.part_processor import sample_parts

def lambda_func(part, topo, points):
    if topo.is_face():
        return topo.normal(points)
    else:
        return None

P, S = sample_parts(parts, num_samples, lambda_func)
print("Sampled points:", P)
```