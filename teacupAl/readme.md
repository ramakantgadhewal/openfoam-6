>blockMesh
- Updated surfacefeatureExtract file for the stl file sections
>surfaceFeatureExtract
- Updated snappyHexMeshDict file on/off for catellatedMesh/snap/addLayers
>snappyHexMesh
- Change patch to wall in \polyMesh\boundary where ever required
>renumberMesh
- Replace polymesh files constant folder from 0.1 folder created by renumberMesh command

## setSet commands
setSet
readline>List //gives all available zones
readline>cellSet fluidSet new boxToCell (-250 -250 -250) (250 250 250) //creates a new zone named "fluidSet" and inculdes all the cells with the bounding box
readline>cellSet fluidSet delete zoneToCell cylinder1 //removes all the cells from zone named "cylinder1"
readline>cellZoneSet fluid new setToCellZone fluidSet //creates a new zone named "fluid" from cellSet "fluidSet"
readline>quit

>splitMeshRegions -cellZonesOnly -overwrite
>chtMultiRegionFoam > solver.log &
>watch -n(10) tail -n(10) solver.log

## Foam to VTK by region
>foamToVTK -region cup
>foamToVTK -region tea
>foamToVTK -region fluid

## Parafoam to creat .foam file by region
>paraFoam -touchAll