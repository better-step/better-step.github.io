<!-- # Topology Module

Describes how geometric entities are connected to form complex 3D structures.

## Key Components

- **Edges:** Boundaries of surfaces tied to 3D curves.
- **Faces:** Surface regions trimmed by loops.
- **Halfedges & Loops:** Fine-grained representation of boundaries.
- **Shells:** Groups of faces forming a boundary.
- **Solids:** Complete enclosed volumes.

### Usage Example

```python
from hdf5_mesh_sampler.topology import Face
import numpy as np

face_data = {...}  # Example dictionary from HDF5
face = Face(face_data)
sample_params = np.linspace(0, 1, 50).reshape(-1,1)
normals = face.normal(sample_params)
print(normals)
```

> Auto-generated API reference appears here via mkdocstrings. -->

# Topology Module

The topology module describes **how geometric entities are connected** to form a valid CAD boundary representation (B-Rep). While the geometry module defines *what* curves and surfaces are, topology defines *how they are stitched together* into faces, shells, and solids, enabling adjacency queries, traversal, and structure-aware sampling.

---

## Concepts

- **Edges**  
  Boundary curves in 3D. An edge references a 3D curve and endpoints (vertices). Edges are typically used through halfedges, which provide direction and connectivity.

- **Halfedges**  
  Directed edge segments used to build boundaries. Halfedges reference:
  - a 2D trimming curve (in surface parameter space),
  - the underlying 3D edge,
  - orientation w.r.t. the edge,
  - optional **mates** (the corresponding halfedge on an adjacent face).

- **Loops**  
  Closed sequences of halfedges that form a boundary of a face. A face can have one **outer loop** and optional inner loops (holes).

- **Faces**  
  Trimmed regions of a surface, bounded by loops. Faces provide access to surface-based operations such as **sampling**, **derivatives**, and **normals** (with correct orientation).

- **Shells**  
  Collections of faces. A shell forms a closed boundary (or part of a boundary) of a solid.

- **Solids**  
  Top-level volumetric entities composed of one or more shells.

---

## Loading topology from an HDF5 part

Topology and geometry are stored per-part in the HDF5 file. The recommended entry point is `read_parts`, which returns a list of part objects with topology already constructed and linked (e.g., `Part → Solid → shells → faces → loops → halfedges`).

```python
from abs.utils import read_parts

parts = read_parts("data/sample_hdf5/Cone.hdf5")
solids = parts[0].Solid.solids

print("Number of shells for the first solid:", len(solids[0].shells))
```
In this example:

- `parts[0]` selects the first part in the file.
- `parts[0].Solid.solids` returns the list of solids in that part.
- Each solid contains one or more `shells`, which in turn contain the boundary faces of the model.

## Adjacency queries

### Finding adjacent faces

Faces can query adjacency through **mated halfedges**. This is useful for segmentation, feature detection, and topology-driven traversal.

```python
from abs.utils import read_parts

parts = read_parts("data/sample_hdf5/Cone.hdf5")
solids = parts[0].Solid.solids

faces = parts[0].Solid.faces
adjacent = faces[0].find_adjacent_faces()

print("Adjacent faces for face 0:", [f.id for f in adjacent])
```
> **Implementation note:** Adjacency depends on correctly populated `mates` links and resolved loop/face references. If you are constructing topology directly from raw HDF5 groups, ensure your build step converts indices into object references (e.g., edges → curves, halfedges → edges/curves, loops → halfedges, faces → loops, etc.).


## Trimming and inside/outside checks

Faces may include 2D trimming curves (`trimming_curves_2d`) that define the valid region of the surface in parameter space. When sampling in UV space, you can filter candidate points to keep only those that lie inside the trimmed face domain.

```python
# uv_points: (N, 2) array of candidate parameter-space samples
mask = face.filter_outside_points(uv_points)
uv_interior = uv_points[mask]
```
This is commonly used when sampling surfaces that are trimmed by loops (e.g., holes, cutouts, and other bounded face regions).

## API summary

### `Edge`
- `get_length()`
- `derivative(points, order=1)`
- `sample(sample_points)`

### `Face`
- `normal(points)`
- `get_area()`
- `derivative(points, order=1)`
- `sample(sample_points)`
- `find_adjacent_faces()`
- `filter_outside_points(uv_points)`

### `Halfedge`
- `get_length()`
- `derivative(points, order=1)`
- `sample(sample_points)`

### `Loop`
- Contains `halfedges`

### `Shell`
- Contains `faces`

### `Solid`
- Contains `shells`
