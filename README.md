# Adaptive Mesh Refinement

OpenFoam® motorBike case with adaptive mesh refinement based on curl(U) or grad(p)

# How to run the case

1. Make sure you have installed latest [OpenFoam® v2012](https://www.openfoam.com/download/)
2. Clone this repository on your machine `git clone https://github.com/airshaper/adaptive-mesh-refinement.git`
3. Adopt `primal_run/system/decomposeParDict` depending on number of cores you have. Change `numberOfSubdomains 6` to number of cores or threads you have available. Also modify `n` under `coeffs` to reflect `numberOfSubdomains` change. For example if you have 12 cores your `n` should be `(3 2 2)` if you have 16 cores for example then `n` should be `(4 2 2)` and so on.
4. Run `./Allrun`

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
