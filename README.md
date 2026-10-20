# KAST: a 3D-Printed, Breathable Forearm Cast

A parametric, lattice-style forearm cast generated in Blender with Geometry Nodes and printed in PLA. The goal is a cast that is strong, ventilated, and customizable to an individual limb, with a longer-term plan to make it an *active* medical device with sensors and electrical stimulation.

<img width="455" height="587" alt="image" src="https://github.com/user-attachments/assets/3136587a-e9be-4f52-b320-33c2f334f25b" />

<!-- Add: screenshot of the finished model in Blender -->

> **Disclaimer:** KAST is a student prototype. It has not been tested on people, it is not a medical device, and it must not be used as a substitute for a cast applied by a clinician.

---

## Why I built this

A family member got a small hairline fracture in her forearm. After the first few days the pain faded, but the complaints about the cast didn't: it was uncomfortable, itchy, and impossible to ventilate. Around the same time my 3D printer arrived. While running test prints to break it in, I wondered if I could print a cast instead: a spiderweb-like structure that is strong but breathable.

I had no prior experience with 3D-printed structures, so this project has been a lot of learning by doing.

## How it works

I didn't want to model the cast by hand. I wanted an algorithm that could generate a custom design for any individual limb.

The design lives in a Blender **Geometry Nodes** setup (Blender 5.0). At a high level, the node graph:

1. Converts the input limb-shaped mesh into a volume and back into a shell (Mesh to Volume, then Volume to Mesh)
2. Reads the shape's bounding box to compute height and angle attributes around the limb
3. Builds a hexagonal (Voronoi-like) cell pattern using Dual Mesh
4. Extrudes and scales the cells to set strut thickness, deleting faces to open the holes
5. Outputs a single printable mesh, with the pattern controlled by position along the limb

<img width="940" height="493" alt="image" src="https://github.com/user-attachments/assets/82b5c75d-8f6e-4bc5-9372-1243f74c9534" />
<!-- Add: screenshot of the node graph -->

## Print settings

| Setting | Value |
|---|---|
| Printer | Bambu Lab A1 |
| Slicer | Bambu Studio |
| Material | PLA Basic |
| Nozzle | 0.4 mm |
| Layer height | 0.16 mm (Optimal profile) |
| Wall loops | 2 |
| Infill | 15%, grid |

<img width="1201" height="727" alt="image" src="https://github.com/user-attachments/assets/3662061c-cdb8-4391-909d-196e58e971f2" />

<!-- Add: slicer preview screenshot -->

### Known printing issues

The raw mesh exported from Geometry Nodes triggered slicer warnings (empty layers and floating regions), which point to non-manifold geometry or thin sections in the lattice. Merging vertices and repairing the mesh before export is part of the workflow. <!-- Edit this once you've written down exactly what fixed it for you -->




## Structural validation

**Status: in progress.**

Planned approach: static finite element analysis (FreeCAD FEM with CalculiX) on a representative section of the cast.

- Material: PLA (assumed Young's modulus about 3500 MPa, Poisson's ratio 0.36, yield strength about 50 MPa)
- Constraint: fixed at one end
- Load: bending and compressive loads representative of everyday use
- Output: von Mises stress, displacement, and safety factor

<!-- Fill in after running the analysis: -->
<!-- - Section analysed: -->
<!-- - Load and direction: -->
<!-- - Mesh element size: -->
<!-- - Max von Mises stress: -->
<!-- - Safety factor (yield / max stress): -->
<!-- - Image of stress plot -->

Limitations to keep in mind: printed PLA is weaker across layers than the isotropic material assumed in the simulation, so results should be treated as optimistic until physical load testing is done.

## Project status

- [x] Parametric lattice design in Blender (Geometry Nodes)
- [x] Slicing and test printing (Bambu Lab A1, PLA)
- [ ] Mesh repair and clean print of the full cast
- [ ] Static FEA of a representative section (FreeCAD)
- [ ] Physical load testing of a printed section
- [ ] Fit testing on a model limb

## Future work

KAST is meant to go beyond a passive cast:

- **Sensing:** embed sensors such as temperature to monitor healing over time and help detect signs of infection.
- **Functional electrical stimulation (FES):** use the lattice openings to place electrodes that stimulate muscles and help offset the muscle atrophy caused by immobilization.

## Repository contents

```
KAST/
├── blender/        # .blend file with the Geometry Nodes setup
├── stl/            # exported printable meshes
├── images/         # screenshots and results
└── README.md
```
<!-- Adjust the folder names to match your repo -->

## How to use

1. Open the `.blend` file in Blender 5.0 or later.
2. Select the input limb object and apply the Geometry Nodes modifier.
3. Adjust the parameters (cell size, strut thickness, etc.).
4. Apply the modifier, clean the mesh (Merge by Distance, check for holes), and export as STL in millimeters.
5. Slice with the settings above.

<!-- Add the actual parameter names from your node group -->

## License

<!-- Pick one, e.g. MIT for code or CC BY 4.0 for the design files -->

## Author

<!-- Your name and a link to contact or your profile -->
