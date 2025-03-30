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
***Description:*** It is used for []. It's supported for []
***Main Functions:***  
* `pre_process_demo_helper_fcns.py`
***Description:***  
***Main Functions:***  
#### Step2: Discretization

#### Step3: 

#### Step4:

#### Step5:

#### Step6:

#--------------
### Tutorials of Use for Each Code Piece  
### Mesh Generation


#--------------
### Examples
1. Linear
