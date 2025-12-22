# Installation

## Overview

Better Step is distributed on PyPI and can be installed with `pip`. The recommended package name is:

- **PyPI package:** `abs-hdf5`

This guide covers installation in a clean Python environment and a quick sanity check to confirm everything is working.

---

## Requirements

- **Python 3.7+**
- A working C/C++ toolchain may be required on some systems if dependencies need to build from source.
- (Recommended) A virtual environment to keep dependencies isolated.

> Note: HDF5 support is provided through Python dependencies (e.g., `h5py`) and will be installed automatically as needed.

---

## Install with pip (recommended)

```bash
pip install abs-hdf5

```

## Verify installation

```bash
python -c "import abs_hdf5; print('abs_hdf5 imported successfully')"
```

If you see a version number without error, the install is successful.

> Tip: We recommend creating a virtual environment for an isolated installation.
