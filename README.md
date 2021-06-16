![AirShaper](images/logo.png?raw=true "AirShaper")

# Adaptive Mesh Refinement

OpenFoam® motorBike case with adaptive volume & surface mesh refinement based on curl(U) or grad(p)

## Description

This repository provides adaptive mesh refinement for both the volume mesh and the surface mesh.
Any field can be used for refinement. In this tutorial, two options have been included:

- grad(p): the pressure gradient
- curl(U): the curl of the velocity field

  Both are multipled by the cell length to "discourage" the algorithm to keep refining the smallest cells.
  The method has 3 major parts:

1. Initial mesh

   SnappyHexMesh is run in two separate steps:

   - castellated mesh: only runs the refinement phase using "baffles" for the surface of the geometry (to avoid the mesh inside the geometry being cut and to keep all cells hexagonal).
     This mesh is stored in `primal_refinement` for use in the mesh refinement phase.
   - snapping phase: this part will cut the mesh (by also having castellated enabled) and snap it to the surface (by setting the surface patch type to `wall`).
     This mesh is used in `primal_run` for the first CFD loop.

2. Refinement field

   Once the first CFD loop has finished, it maps the fields onto the stored castellated mesh (in `primal_refinement`) and calculates the refinement field (`refVal`), for which the lower limit value should be set manually in `dynamicMeshDict`.

3. Mesh refinement

   The refinement is then applied to the castellated mesh of the first loop. Once refinement is done, the castellated mesh is copied to `primal_run` where the snapping phase is executed again to snap the refined mesh to the surface.
   This mesh is then used for the second CFD run.

Flow Chart
![Flow Chart](images/flow_chart.png?raw=true "Flow Chart")

## Conclusions

This method allows for mesh refinement of both the volume and the surface mesh.

In contrast to other OpenFOAM® based methods that refine on the surface, the mesh is re-snapped after refinement, which improves the correspondence to the real geometry.

Adaptive mesh refinement can greatly reduce the computational cost, as the mesh is applied in a more efficient way.

Any given field can be created to steer the mesh refinement.
The gradient of pressure (useful for airfoils for example) and curl of the velocity field (useful for wake refinement for example) have been included.

We definitely welcome any contribution to further improve this method (for example, would it be easier to just use refineMesh instead of pimpleFoam?).

## How to run the case

1. Make sure you have installed latest [OpenFoam® v2012](https://www.openfoam.com/download/)
2. Clone this repository on your machine `git clone https://github.com/airshaper/adaptive-mesh-refinement.git`
3. Adopt `primal_run/system/decomposeParDict` depending on number of cores you have. Change `numberOfSubdomains 6` to number of cores or threads you have available. Also modify `n` under `coeffs` to reflect `numberOfSubdomains` change. For example if you have 12 cores your `n` should be `(3 2 2)` if you have 16 cores for example then `n` should be `(4 2 2)` and so on.
4. Go to `cd ./primal_run` then run `./Allrun`

## Results

Non-refined mesh slice
![Non-refined mesh](images/motorBike_before_refinement_slice.png?raw=true "Non-refined mesh")

Refined mesh slice with curl(U) (notice how curl(U) refines the wake as well)
![Refined Mesh with curl(U)](images/motorBike_after_refinement_slice_curlU.png?raw=true "Refined Mesh with curl(U)")

Refined mesh slice with grad(p) (notice how grad(p) mostly refines the surface)
![Refined Mesh with grad(p)](images/motorBike_after_refinement_slice_gradp.png?raw=true "Refined Mesh with grad(p)")

Zoomed Non-refined mesh slice
![Zoomed Non-refined mesh)](images/motorBike_before_refinement_slice_zoomed.png?raw=true "Zoomed Non-refined mesh")

Zoomed Refined mesh slice with curl(U)
![Zoomed Refined Mesh with curl(U)](images/motorBike_after_refinement_slice_zoomed_curlU.png?raw=true "Zoomed Refined Mesh with curl(U)")

Zoomed Refined mesh slice with grad(p)
![Zoomed Refined Mesh with grad(p)](images/motorBike_after_refinement_slice_zoomed_gradp.png?raw=true "Zoomed Refined Mesh with grad(p)")

Non-refined surface mesh
![Non-refined mesh](images/motorBike_before_refinement_surface.png?raw=true "Non-refined surface mesh")

Refined surface mesh based on curl(U)
![Refined surface mesh based on curl(U)](images/motorBike_after_refinement_surface_curlU.png?raw=true "Refined surface mesh based on curl(U)")

Refined surface mesh based on grad(p)
![Refined surface mesh based on grad(p)](images/motorBike_after_refinement_surface_gradp.png?raw=true "Refined surface mesh based on grad(p)")

Custom calculated field refVal based on curl(U)
![Custom calculated field refVal based on curl(U)](images/motorBike_before_refinement_slice_refVal_curlU.png?raw=true "Custom calculated field refVal based on curl(U)")

Custom calculated field refVal based on grad(p)
![Custom calculated field refVal based on grad(p)](images/motorBike_before_refinement_slice_refVal_gradp.png?raw=true "Custom calculated field refVal based on grad(p)")
