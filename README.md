

# Better Step
[![PyPI version](https://img.shields.io/pypi/v/abs-hdf5.svg)](https://pypi.org/project/abs-hdf5/)
<!-- [![image](https://img.shields.io/conda/vn/conda-forge/HDF5MeshSampler.svg)](https://anaconda.org/conda-forge/HDF5MeshSampler) -->

## Description

Better Step is designed to unlock CAD data for research and industry. By converting proprietary STEP files into an open HDF5-based format, Better Step enables efficient processing on large-scale computing clusters, eliminating the need for expensive proprietary licenses.

## Key Features

- **Open Format:** Proprietary CAD files are converted into a widely accessible HDF5 format.
- **Comprehensive Representation:** Captures both the geometry (curves, surfaces) and topology (solids, shells, faces, loops) of CAD models.
- **Robust Sampling Methods:** Provides reliable methods to compute surface normals, detect sharp features, and generate point clouds for machine learning applications.
- **Extensible API:** Easily integrate the library into Python workflows.
- **Command-Line Interface (CLI):** Supports batch processing for streamlined integration in automated pipelines.

# Installation

Better Step is distributed on PyPI and can be installed with `pip`. The recommended package name is:

- **PyPI package:** `abs-hdf5`

This guide covers installation in a clean Python environment and a quick sanity check to confirm everything is working.

---

## Requirements

- **Python 3.7+**
<!-- - A working C/C++ toolchain may be required on some systems if dependencies need to build from source. -->
- (Recommended) A virtual environment to keep dependencies isolated.

> Note: HDF5 support is provided through Python dependencies (e.g., `h5py`) and will be installed automatically as needed.

---

## Install with pip (recommended)

```bash
pip install abs-hdf5
```

## Usage

Better Step provides both a Python API and a command‑line interface for processing CAD data.

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

## Documentation

Detailed documentation is available on our website. It includes:

- **Installation Guide**
- **Usage Examples**
- **API Documentation:** Covers modules like Geometry, Topology, Sampling, and Visualization.

Visit our website: [better-step.github.io](https://better-step.github.io)


## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Acknowledgments

Better Step is developed and maintained by a dedicated team. For a complete list of contributors, please see the Authors page.
