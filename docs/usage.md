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

You can use `sample_parts` to draw samples from one or more CAD parts.

The function returns:

- **`P`** – the sampled 3D points  
- **`S`** – per-sample auxiliary data computed during sampling  

The behavior of `sample_parts` is controlled by two user-defined functions:

- `face_func(face, points)` – evaluated for samples drawn on faces  
- `edge_func(edge, points)` – evaluated for samples drawn on edges  

These functions allow you to compute arbitrary per-sample attributes such as:

- Surface normals  
- Curvature features  
- Face/edge IDs  
- Custom geometric descriptors  

Each function receives:

- The geometric entity (`face` or `edge`)  
- The sampled points on that entity  

and must return the auxiliary data associated with those samples.

```python
from abs.part_processor import sample_parts

def face_func(face, points):
    return face.normal(points)

P, S = sample_parts(parts, num_samples, face_func)
print("Sampled points:", P)
```

## Advanced Use Cases

- **Learning Normals:** Use sample data to train or validate normal estimation.
- **Detecting Sharp Features:** Analyze curves to detect geometric edges.
- **Labeling Geometry:** Assign labels based on detected primitive types.
