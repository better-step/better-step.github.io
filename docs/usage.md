# Usage

## Overview

<!-- Learn how to use the Better Step library to process CAD files. Load shapes from HDF5 files, sample geometry, and utilize the CLI for batch operations. -->
This page demonstrates the core workflows in **ABS-HDF5**: working with CAD models stored in the HDF5 format, sampling geometry for downstream tasks (e.g., point clouds and normals), and using the command-line interface to run the same steps in batch pipelines. The examples below are intended to be minimal and reproducible, start here, then expand to larger datasets and cluster-scale processing.

## Basic Workflow

### Loading a CAD File

```python
from abs.utills import read_parts
parts = read_parts("data/sample_hdf5/Box.hdf5")
print("Loaded parts:", parts)
```
### Reading the Meshes
```python
from abs.utills import read_meshes
meshes = read_meshes("data/sample_hdf5/Box.hdf5")
```

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

<!-- ### Using the CLI

```bash
python -m hdf5_mesh_sampler.cli --input data/sample_hdf5/Box.hdf5 --output output_directory
``` -->

## Advanced Use Cases

- **Learning Normals:** Use sample data to train or validate normal estimation.
- **Detecting Sharp Features:** Analyze curves to detect geometric edges.
- **Labeling Geometry:** Assign labels based on detected primitive types.

> See the example notebooks in the Examples section for more details.
