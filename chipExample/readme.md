## UNV to Multiregion
> ideasUnvToFoam cfd554mesh3.unv
- Change patch to wall in \polyMesh\boundary where ever required
>renumberMesh
- Replace polymesh files constant folder from 0.1 folder created by renumberMesh command
> splitMeshRegions -cellZones -overwrite
>chtMultiRegionFoam > solver.log &
>watch -n(10) tail -n(10) solver.log

## Foam to VTK by region
>foamToVTK -region air_fluid
>foamToVTK -region chip_solid
>foamToVTK -region heat_sink_solid

## Parafoam to creat .foam file by region
>paraFoam -touchAll