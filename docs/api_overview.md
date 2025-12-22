<!-- # Better Step API Overview

The Better Step API is divided into several core modules. Each module provides the functionality required to work with different aspects of CAD data processing.

## Modules Include

- **Geometry:** Classes and methods to represent and manipulate curves and surfaces.
- **Topology:** Structures for organizing relationships between geometric entities (edges, faces, loops, shells, and solids).
- **Sampling:** Functions for extracting point clouds, computing normals, and generating features.
<!-- - **Visualization:** Tools to render and inspect CAD models and sampling results. -->

# Better Step API Overview

The Better Step API is organized into a small set of core modules that cover the full CAD processing workflow: **load** CAD parts from HDF5, **inspect** geometry and topology, and **sample** data for downstream applications such as machine learning, feature detection, and large-scale analysis.


## Modules

- **Utilities (`abs.utils`)**  
  Entry points for loading data from HDF5, including helpers such as `read_parts` (and `read_mesh` when mesh data is available).

- **Geometry (`abs.curve`, surfaces)**  
  Classes and methods to represent and evaluate geometric primitives such as curves and surfaces (e.g., B-splines), including sampling and derivatives.

- **Topology (`abs.topology`)**  
  B-Rep connectivity and structure: `Edge`, `Halfedge`, `Loop`, `Face`, `Shell`, and `Solid`. Provides traversal and queries such as face adjacency and trimming-aware filtering.
  
- **Sampling ( `abs.part_processor`)**  
  Tools to extract point samples and derived attributes (e.g., normals, sharp features). Includes batch-friendly routines like `sample_parts`, where a user-defined `lambda_func` can compute arbitrary per-sample outputs alongside points.

---

## Typical workflow

1. **Load parts** from an HDF5 file (`read_parts`)
2. **Traverse topology** (solids → shells → faces → loops → halfedges)
3. **Evaluate geometry** (curves/surfaces) and **sample** points and attributes
4. **Scale up** using the CLI or batch scripts for dataset processing
