# FE-model
## Contents
- `FEM_Model_Description.md`: Key simulation parameters
- `Sample_Data.csv`: Example magnetic field data 

Step 1: Launch ANSYS Electronics Desktop & Initialize Project
Open ANSYS Electronics Desktop.

Create new project:
File → New → Project

Insert Maxwell 3D design:
Project → Insert Maxwell 3D Design

Step 2: Configure Solution Type
Set solver:
Maxwell 3D → Solution Type → Transient

Why Transient? Pulsed current requires time-domain analysis.

Enable symmetry:
Model → Symmetry → Symmetry Region

Select YZ-plane (symmetric about armature center).

Set symmetry type: Anti-Symmetric (enforces B<sub>normal</sub> = 0 for EM fields).
Step 3: Define Geometry
*All units in mm. Model uses 1/2 symmetry (X≥0 plane).*
Global Coordinate System:

Armature center at (0, 10, 0) (shifted from original (7.5, 10, 0) for symmetry).

a) Backing/Guide Rails (Copper)
Shape: Rectangular prism

Dimensions:

Length (X): 1992.5 (full rail: 2000 mm; half-model: 0 → 1992.5 after symmetry shift)

Width (Y): 5

Height (Z): 10
b) Armature (Copper Core + Steel Contact Liners)
Core Dimensions: 19 (L) × 10 (W) × 19 (H)

Position: (0, 5.5, 0) to (9.5, 14.5, 19)
*(Y: 5.5→14.5 ensures contact liners fit at Y=5-5.5 and 14.5-15)*

Contact Liners (Steel Alloy):

Thickness: 0.5 mm (sides of armature)

Left Liner: (0, 5, 0) to (9.5, 5.5, 19)

Right Liner: (0, 14.5, 0) to (9.5, 15, 19)
Step 4: Assign Materials
Rails & Armature Core:

Select objects: Rail_Left, Rail_Right, Armature_Core

Assign material: Edit → Assign Material → copper

Contact Liners:

Select objects: Contact_Liner_Left, Contact_Liner_Right
Step 5: Define Excitations (Current Input)
Use experimentally measured pulsed current waveform (e.g., from CSV file).

Create Current Source:

Select face at X=0 for Rail_Left:
Maxwell 3D → Excitations → Assign → Current → Solid

Name: I_Left

Value: +<function> (positive current)

Repeat for Rail_Right (same face at X=0):

Name: I_Right

Value: -<function> (negative current)
Step 6: Set Boundary Conditions & Mesh
Region Boundary:
Draw → Region → Padding: 500 mm (creates air domain).

Mesh Operations:

Refine mesh near armature/rails:
Maxwell 3D → Mesh Operations → Assign → On Selection → Length Based

Objects: Armature_Core, Contact_Liner*, Rail*

Max element length: 2 mm
Step 7: Configure Transient Setup
Analysis Setup:
Maxwell 3D → Analysis Setup → Add Solution Setup

Stop Time: Match current pulse duration (e.g., 2 ms).

Time Step: 1e-6 s (adjust based on pulse rise time).

Save Fields:

Enable field recording at critical time points.
Step 8: Validate & Run Simulation
Validate model: ✓ Validation Check (fix any errors).

Run simulation: Analyze All.
Step 9: Post-Processing
Plot current/voltage:
Results → Create Transient Report → Rectangular Plot

Visualize EM fields:
Fields → Fields → Mag_B / Vector_H

Force calculation (optional):
Parameters → Assign → Force (select armature).

