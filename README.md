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
### Purpose of the code   
This code is written to [to be written]. The final goal here is to see how each part of the code fit together and what ...

### Map  
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
### Map 2
Pre-processing
 (pre_process.py)
       │
       ▼
Discretization (shape functions & quadrature)
 (discretization.py)
       │
       ▼
Element-level computations
 (local_element.py)
       │
       ▼
Global assembly
 (assemble_global.py)
       │
       ▼
Solver (Newton–Raphson iterations)
 (solver.py)
       │
       ▼
Visualization & Post-processing
 (visualize.py)

### Steps

#### Step1: Preprocessing and Mesh Generation
***Pieces of code:*** `pre_process.py` and `pre_process_demo_helper_fcns.py`.  
* `pre_process.py`  
***Description:*** It is used for generating structured rectangular meshes for finite element analysis (FEA). It supports 3-node and 6-node triangular meshes, and 4-node and 8-node quadrilateral meshes.  
***Main Functions:***  
  - `generate_rect_mesh_2d`: Generates rectangular meshes based on provided dimensions, subdivisions, and element types.

* `pre_process_demo_helper_fcns.py`  
***Description:*** It provides auxiliary functions to visualize meshes, calculate Gauss integration points, and interpolate scalar fields onto Gauss points.  
***Main Functions:***  
  - `plot_mesh_2D`: Visualizes the mesh with labeled nodes and elements.
  - `get_all_mesh_gauss_pts`: Computes the physical coordinates of Gauss points.
  - `interpolate_scalar_to_gauss_pts`: Interpolates scalar functions at Gauss integration points.

#### Step2: Discretization (Shape Functions & Quadrature)
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

#### Step3: Element-Level Computations
***Pieces of code:*** `local_element.py`  
***Description:*** Computes local stiffness matrices, internal force (residual) vectors, and distributed loads for hyperelastic finite element problems.  
***Main Functions:***
  - `element_residual`: Calculates element residual vectors for hyperelastic materials.
  - `element_distributed_load`: Computes element-level load vectors from distributed surface tractions.

#### Step4: Global Assembly
***Pieces of code:*** `assemble_global.py`  
***Description:*** Assembles local element-level computations into global stiffness matrices, residual vectors, and traction vectors.  
***Main Functions:***
  - `global_stiffness` and `global_stiffness_sparse`: Assemble the global stiffness matrix from local element stiffness matrices.
  - `global_residual`: Assembles global residual vectors.
  - `global_traction`: Constructs global load vectors due to external traction.

#### Step5: Solver
***Pieces of code:*** `solver.py` and `solver_demo_helper_functions.py`  
* `solver.py`  
***Description:*** Solves nonlinear finite element equations for hyperelastic materials using Newton–Raphson iterative methods.  
***Main Functions:***
  - `hyperelastic_solver`: Performs incremental loading and solves nonlinear equations iteratively to determine nodal displacements.

* `solver_demo_helper_functions.py`  
***Description:*** Provides diagnostic tools and visualizations to analyze solver performance and stiffness matrix properties.  
***Main Functions:***
  - `compute_bandwidth`, `compute_condition_number`, `compute_sparsity`: Diagnose stiffness matrix properties.
  - `analyze_and_visualize_matrix`: Visualizes the stiffness matrix sparsity pattern and evaluates its numerical characteristics.

#### Step6: Visualization & Post-Processing
***Pieces of code:*** `visualize.py`  
***Description:*** Offers visualization functions to plot and animate finite element analysis results, specifically mesh deformations and displacement fields.  
***Main Functions:***
  - `plot_mesh_2D`: Plots initial and deformed meshes along with displacement magnitude.
  - `make_deformation_gif`: Creates animated GIFs depicting mesh deformation progression through loading steps.

#--------------
### Tutorials of Use for Each Code Piece  
### Mesh Generation


#--------------
### Examples
1. Linear
