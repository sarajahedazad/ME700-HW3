[![codecov](https://codecov.io/gh/sarajahedazad/ME700-HW3/graph/badge.svg?token=DRF09dG4CR)](https://codecov.io/gh/sarajahedazad/ME700-HW3)
[![Run Tests](https://github.com/sarajahedazad/ME700-HW3/actions/workflows/tests.yml/badge.svg)](https://github.com/sarajahedazad/ME700-HW3/actions/workflows/tests.yml)


# ME700-HW3

### Conda environment, install, and testing

Note: To be completed.

```bash
conda create --name finite-element-analysis-env python=3.12.9
```

```bash
conda activate finite-element-analysis-env
```

```bash
python --version
```

```bash
pip install --upgrade pip setuptools wheel
```

```bash
pip install -e .
```

```bash
pytest -v --cov=finiteelementanalysis --cov-report term-missing
```
---  
### Purpose of the code    
This repository is designed to solve finite element analysis (FEA) cproblems. It systematically generates meshes, computes shape functions, assembles local element matrices into global systems, and iteratively solves nonlinear equations using the Newton–Raphson method. Comprehensive visualization tools are included to analyze and interpret the results, providing clear insights into mesh deformations and stress distributions.

---
### Map 1
       pre_process: generate mesh
              ↓
       solver: for each load step
         → assemble_global:
             - local_element computations
             - sum into global K, R
         → solve system
         → check convergence
              ↓
       visualize: final or incremental results

<p align="center">
<img src="https://github.com/sarajahedazad/ME700-HW3/blob/main/figures/Block%20diagram.png" width="250">
</p>

------  
### Steps    

✔️ **Step1: Preprocessing and Mesh Generation**  
***Pieces of code:*** `pre_process.py` and `pre_process_demo_helper_fcns.py`.  
* `pre_process.py`  
***Description:*** It is used for generating meshes for finite element analysis (FEA). It supports 3-node and 6-node triangular meshes, and 4-node and 8-node quadrilateral meshes.  
***Main Functions:***  
  - `generate_rect_mesh_2d`: Generates rectangular meshes based on provided dimensions, subdivisions, and element types.

* `pre_process_demo_helper_fcns.py`  
***Description:*** It provides auxiliary functions to visualize meshes, calculate Gauss integration points, and interpolate scalar fields onto Gauss points.  
***Main Functions:***  
  - `plot_mesh_2D`: Visualizes the mesh with labeled nodes and elements.
  - `get_all_mesh_gauss_pts`: Computes the physical coordinates of Gauss points.
  - `interpolate_scalar_to_gauss_pts`: Interpolates scalar functions at Gauss integration points.



✔️ **Step2: Discretization (Shape Functions & Quadrature)**   
***Pieces of code:*** `discretization.py` and `discretization_demo_helper_fcns.py`.  
* `discretization.py`  
***Description:*** Defines shape functions, their derivatives, and Gauss integration rules for different finite element types.  
***Main Functions:***
  - `element_info`: Provides element-specific details (dimension, DOFs, number of nodes).
  - `shape_fcn` and `shape_fcn_derivative`: Computes shape functions and their derivatives for various element types.
  - `integration_info`: Supplies Gauss integration points and weights.

* `discretization_demo_helper_fcns.py`  
***Description:*** Demonstrates discretization concepts via visualization of interpolated fields and element mappings.  
***Main Functions:***
  - `interpolate_field_natural_coords_single_element`: Interpolates scalar fields within an element.
  - `plot_interpolate_field_natural_coords_single_element`: Visualizes interpolations in natural coordinates.
  - `visualize_isoparametric_mapping_single_element`: Visualizes isoparametric mappings.



✔️ **Step3: Element-Level Computations**   
***Pieces of code:*** `local_element.py`    
***Description:*** Computes local stiffness matrices, internal force (residual) vectors, and distributed loads for finite element problems.  
***Main Functions:***
  - `element_residual`: Calculates element residual vectors for hyperelastic materials.
  - `element_distributed_load`: Computes element-level load vectors from distributed surface tractions.
  - `element_stiffness`: Computes the element stiffness matrix.



✔️ **Step4: Global Assembly**   
***Pieces of code:*** `assemble_global.py`    
***Description:*** Assembles local element-level computations into global stiffness matrices, residual vectors, and traction vectors.  
***Main Functions:***
  - `global_stiffness` and `global_stiffness_sparse`: Assemble the global stiffness matrix from local element stiffness matrices.
  - `global_residual`: Assembles global residual vectors.
  - `global_traction`: Constructs global load vectors due to external traction.



✔️ **Step5: Solver**   
***Pieces of code:*** `solver.py` and `solver_demo_helper_functions.py`  
* `solver.py`  
***Description:*** Solves nonlinear finite element equations using Newton–Raphson iterative methods.  
***Main Functions:***
  - `hyperelastic_solver`: Performs incremental loading and solves nonlinear equations iteratively to determine nodal displacements.

* `solver_demo_helper_functions.py`  
***Description:*** Provides diagnostic tools and visualizations to analyze solver performance and stiffness matrix properties.  
***Main Functions:***
  - `compute_bandwidth`, `compute_condition_number`, `compute_sparsity`: Diagnose stiffness matrix properties.
  - `analyze_and_visualize_matrix`: Visualizes the stiffness matrix sparsity pattern and evaluates its numerical characteristics.



✔️ **Step6: Visualization & Post-Processing**   
***Pieces of code:*** `visualize.py`  
***Description:*** Offers visualization functions to plot and animate finite element analysis results, specifically mesh deformations and displacement fields.  
***Main Functions:***
  - `plot_mesh_2D`: Plots initial and deformed meshes along with displacement magnitude.
  - `make_deformation_gif`: Creates animated GIFs depicting mesh deformation progression through loading steps.

---
***Example***: We have a 2D cantilever of dimensions L×H that is fixed at one end and is exposed to traction at the other end. We want to look at linear elasticity here.

```
'''
In this first numerical example, we demonstrate how to compute the small-strain solution for a 2D isotropic linear elastic medium.
Specifically, we consider a cantilever beam modeled as a two-dimensional domain with dimensions L×H.
'''
# importing modules
from finiteelementanalysis import pre_process as pre
from finiteelementanalysis import pre_process_demo_helper_fcns as pre_demo
from finiteelementanalysis.solver import hyperelastic_solver
import numpy as np
import matplotlib.pyplot as plt

'''Preprocess'''
#---------Defining a mesh------------
x_orig, y_orig = 0, 0 # coordinates of the left-below corner of a rectangle
L = 200. # Length
H = 20. # Height
Nx = 50 # Number of cells in the x direction
Ny = 5 # Number of cells in the y direction
ele_type = 'D2_nn3_tri' # Type of element for teh mesh
num_gauss_pts = 1

# Coordinates and connectivities in a mesh 
coords, connect = pre.generate_rect_mesh_2d(ele_type, x_orig, y_orig, x_orig + L, x_orig + H, Nx, Ny)


#-------Defining Boundaries of the Mesh--------
# With the following method, boundary nodes and edges will be dictionaries with keys: right, left, bottom, top
boundary_nodes, boundary_edges = pre.identify_rect_boundaries(coords, connect, ele_type, x_orig, x_orig + L, y_orig, y_orig + H)
# Fix left boundary: both u_x and u_y = 0.
fixed_nodes = pre.assign_fixed_nodes_rect(boundary_nodes, "left", 0.0, 0.0)
# Assign distributed load on the right boundary (uniform on the x direction, 0 on y direction)
q = 10.0
dload_info = pre.assign_uniform_load_rect(boundary_edges, "right", q, 0.0)

#-------Material Properties----------
mu = 10
kappa = 100
material_props = np.array([mu, kappa])


'''Solving'''
# Solving for a hyperelastic material
nr_num_steps = 5
nr_print = True

displacements_all, nr_info_all = hyperelastic_solver(material_props, ele_type, coords.T, connect.T, fixed_nodes, dload_info, nr_print, nr_num_steps, nr_tol=1e-9, nr_maxit=30)


'''Visualization and PostProcessing'''
# Visualizing the mesh
fname = "mesh_2D.png"
pre_demo.plot_mesh_2D(fname, ele_type, coords, connect)

# TO BE WRITTEN



```


---
### Confusion
Question: Look at this from `tutorial_sparse_solver.ipynb`:
```
fixed_nodes = pre.assign_fixed_nodes_rect(boundary_nodes, "left", 0.0, 0.0)
# Assign distributed load on the right boundary
q = 10.0
dload_info = pre.assign_uniform_load_rect(boundary_edges, "right", q, 0.0)
```   
How do you know whether to apply a boundary condition to the "nodes" or "edges"?
