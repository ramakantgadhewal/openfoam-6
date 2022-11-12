## Run foam with log file
laplacianFoam > solver.log &

## To view the end of log file while running
watch -n(10) tail -n(10) solver.log

## Al Thermal Diffucivity:
Thermal Condutivity(k) = 237 W/m K = 237 J/s m K
Density(rho) = 2710 Kg/m3
Specific heat (Cp) = 900 j/Kg K 
So Thermal diffusivity alpha = k/rho Cp = 237/(2710*900) = 9.7e-5 m2/s

## Porcelain Thermal Diffucivity:
Thermal Condutivity(k) = 1.5 W/m K = 1.5 J/s m K
Density(rho) = 2300 Kg/m3
Specific heat (Cp) = 1.07e-3 j/Kg K 

So Thermal diffusivity alpha = k/rho Cp = 1.5/(2300*1.07e-3) = 6.1e-7 m2/s

## Foam to VTK by region
>foamToVTK -region air_fluid
>foamToVTK -region air_fluid -time 20:100 //one specified time rage fromspecific region
>foamToVTK -region chip_solid
>foamToVTK -region heat_sink_solid

## Parafoam to creat .foam file by region
>paraFoam -touchAll

## UNV to Multiregion
> ideasUnvToFoam cfd554mesh3.unv
> Change patch to wall in \polyMesh\boundary where ever required
> renumberMesh (run this if required and replace the polymesh folder)
> splitMeshRegions -cellZones -overwrite

## Foam tutorial functions
. ${WM_PROJECT_DIR:?}/bin/tools/CleanFunctions  
cleanCase0
restore0Dir
foamToEnsight -noZero
foamToVTK

## Mesh Quality
Recommended values for meshing:
### Angle 
The angle of mesh edges < 18°
### Quality of Mesh
Mesh min. quality should be > 0.3
### Aspect ration
Length to Width ratio(aspect ratio) of cells, 
	when the flow is only in one direction.
		for Single precision solver < 1000
		for double-precision solved <10000
	Otherwise
		it should be < 100

	And at the interfaces 
		it should be < 10
### Skewness
Most simulations will run just fine up to a skewness of 8 to 12.
Skewness is defined as the difference between the shape of the cell and the shape of an equilateral cell of equivalent volume. 
Highly skewed cells can decrease accuracy and destabilize the solution. For example, optimal quadrilateral meshes will have vertex 
angles close to 90 degrees, while triangular meshes should preferably have angles of close to 60 degrees and have all angles less than 
90 degrees. A general rule is that the maximum skewness for a triangular/tetrahedral mesh in most flows should be kept below 0.95, 
with an average value that is less than 0.33. A maximum value above 0.95 may lead to convergence difficulties and may 
require changing the solver controls, such as reducing under-relaxation factors and/or switching to the pressure-based coupled solver.

Aspect ratio is a measure of the stretching of the cell. As discussed in Section  6.1.3, for highly anisotropic flows, extreme 
aspect ratios may yield accurate results with fewer cells. Generally, it is best to avoid aspect ratios in excess of 5:1 in the bulk 
flow (away from the walls). The quadrilateral/hexahedral/wedge cells inside the boundary layer, on the other hand, can be stretched to 
aspect ratio of up to 10:1 in most cases. With regard to the stability of the flow solution, it can 
go as high as possible; however, with regard to the stability of the energy solution, the maximum aspect ratio should be kept below 35:1.

Squish index is computed for cells using the vector from the cell centroid to each of its faces and the corresponding face area vector. 
The worst cells will have a cell squish index close to 1, with better cells closer to 0. For tetrahedral meshes, you can use either 
skewness or cell squish index to measure the mesh quality. Skewness information is not available for polyhedral meshes, so you must 
rely on the cell squish index and an additional index for the face squish (which is computed using the vector connecting the centroids 
of adjacent cells). A good rule of thumb is that the maximum 
skewness for tetrahedral cells should less be than 0.95. The maximum cell squish index for all types of cells should be less than 0.99.

## laplacian mesh shrinker
When the mesh skewness is high try this method

https://www.openfoam.com/documentation/guides/latest/doc/guide-meshing-snappyhexmesh-layers.html

## Turbulent length and epsilon value
Supply:
Re = 200000
turbulence intensity I = 0.035
I = 0.16*Re^-1/8 = 0.16*200000^-0.125 = 0.035
Mean flow velocity U = 5.32 m/s
turbulent energy k = 0.052
k = 3/2 (UI)^2 = 3/2*(5.32*0.035)^2 = 0.052

turbulent length l = 0.0228
l = 0.4 times the initial boundary layer thickness 
boundary layer thickness = 0.37 * x / Re^(1/5) = 
(OR)
l = 0.038 * dh = 0.038 * 0.6 = 0.0228

epsilon = 0.085
epsilon = Cmu^(3/4) * K^(3/2) / l
Cmu = 0.09
So epsilon = 0.09^(3/4) * 0.052^(3/2) / 0.0228 = 0.085


Return
Re = 156000
turbulence intensity I = 0.036
Mean flow velocity U = 2.32 m/s
turbulent energy k = 0.010

turbulent length l = 0.04
epsilon = 0.0041

setSet Commands:
================

## Tobi's instaruction

setSet
readline>cellSet fluidSet new
readline>cellSet fluidSet add box
readline>cellSet fluidSet add boxToCell (-160 -10 -85) (310 300 185)
readline>cellSet fluidSet delete zoneToCell teacup2zone
readline>cellZoneSet fluid new setToCellZone fluidSet
readline>quit

## as per opefoamwiki.net

setSet
readline>List //gives all available zones
readline>cellSet fluidSet new boxToCell (-1 -1 -1) (1 1 1) //creates a new zone named "fluidSet" and inculdes all the cells with the bounding box
readline>cellSet fluidSet delete zoneToCell cylinder1 //removes all the cells from zone named "cylinder1"
readline>cellZoneSet fluid new setToCellZone fluidSet //creates a new zone named "fluid" from cellSet "fluidSet"
readline>quit

>splitMeshRegions -cellZonesOnly -overwrite

## Tobi's instruction for snappyHexMesh
Overview
========
+ This is a template case with single inlet and outlet
+ Setup to run the simpleFoam solver
+ The case is designed to be meshed with snappyHexMesh
+ snappyHexMesh is setup to use a single trisurface file named CAD.obj
+ Copy the CAD.obj file to the constant/triSurface directory
+ The CAD.obj should contain an inlet and outlet region to create the relevant
  patches in the mesh

Background Mesh
===============
+ The user should establish the bounds of their CAD.obj file
+ The blockMeshDict file contains a backgroundMesh subditionary
+ For internal flows, where CAD.obj describes the external boundary, set xMin,
  xMax, etc to be beyond the CAD.obj bounds
+ For external flows, the background mesh can define the external boundary by
  uncommenting entries, e.g. left, in the boundary section of blockMeshDict
+ Set background mesh density with xCells, yCells, zCells
+ Run blockMesh

Castellated Mesh
================
+ In the snappyHexMeshDict file, replace <inletPatch> with the name of the inlet
  region in the CAD.obj file
+ Replace <outletPatch> with the name of the outlet region
+ run snappyHexMesh to obtain a castellatedMesh
+ Review the mesh; modify refinement levels and regenerate the mesh as required
  (levels are set in refinementSurfaces and refinementRegions)

Snapped Mesh
============
+ In snappyHexMeshDict, set castellatedMesh off; snap on;
+ Run the snapping phase of snappyHexMesh
+ Review the mesh

Layers
======
+ To add layers to the mesh along wall boundary patches...
+ Switch on addLayers; switch snap off;
+ Run snappyHexMesh
+ The number of layers can be changed by modifying nSurfaceLayers

Initialisation
==============
+ In the field files in the 0 directory, set inlet values
+ For example, in 0/U, set the inlet velocity Uinlet
+ Set the viscosity in constant/transportProperties


## Reynold number for Air

Re = dh * U * rho / mu

vlues for tea cup example
rho = 1.2 kg/m3
mu = 1.84e-5 kg/m-s
U = 1 m/s
dh = 4 * area / Hydraulic perimeter
	= 4* (0.45*0.29)/2.2
	= 0.24 m
So, Re 	= 0.24 * 1 * 1.2/1.84e-5
		= 15474

========================
--------------------------------------------------------------------------
There are not enough slots available in the system to satisfy the 10
slots that were requested by the application:

  chtMultiRegionSimpleFoam

Either request fewer slots for your application, or make more slots
available for use.

A "slot" is the Open MPI term for an allocatable unit where we can
launch a process.  The number of slots available are defined by the
environment in which Open MPI processes are run:

  1. Hostfile, via "slots=N" clauses (N defaults to number of
     processor cores if not provided)
  2. The --host command line parameter, via a ":N" suffix on the
     hostname (N defaults to 1 if not provided)
  3. Resource manager (e.g., SLURM, PBS/Torque, LSF, etc.)
  4. If none of a hostfile, the --host command line parameter, or an
     RM is present, Open MPI defaults to the number of processor cores

In all the above cases, if you want Open MPI to default to the number
of hardware threads instead of the number of processor cores, use the
--use-hwthread-cpus option.

Alternatively, you can use the --oversubscribe option to ignore the
number of available slots when deciding the number of processes to
launch.
--------------------------------------------------------------------------
## checkMesh

checkMesh -help
checkMesh -allGeometry -allTopology -writeAllFields -writeSets vtk
----------------------------------------------------

## STL 

To add name of the solid in a stl file

The below one adds "hsair3walls" as solid name in the file "hsair3walls.stl"

sed -i '1 s/^.*$/solid hsair3walls/' hsair3walls.stl
---------------

## setSet to remove bad cells

cellSet c0 new faceToCell zeroVolumes any
cellSet c0 add faceToCell wrongFaces any
cellSet c0 invert
quit

-> subsetMesh c0 -overwrite
-> then rename the empty patch in boundary file
----------------------

## Run Parallel

. ${WM_PROJECT_DIR:?}/bin/tools/RunFunctions

decomposePar -allRegions

runParallel chtMultiRegionFoam

reconstructPar -allRegions

## To generate residuals from a log file

foamLog solver4.log

-> this command creates folder callled log that has all residuals


## Explore WSL file system in windows explorer

-> run the below from WSL terminal

>"explorer.exe ."

## CheckMesh details

#### Cells with small determinant
  -> Volume is less compared to face area
  -> They are planar and collinear

#### Concave cells
The errors are about the cells that result from the way blockMesh connects the two blocks.

Let me try some ascii, ignore the grey x'es.

o----o
|xxx |
|xxx o---o
|xxx |xx |
o----o---o


You see two cells, that are connected. The left cell has five nodes (the four corner nodes and the hanging node), whereas the right cell has only four. checkMesh will report the left cell as concave, because there are faces with parallel face normal vectors (the green and the blue face). However, there is nothing wrong with this cell.

If we - manually modify the location of the nodes, we get rid of the error

o----o
| xxx \
| xxxx o--o
| xxx / xs |
o----o-----o

Now we have moved the hanging node with the result, that the face normal vectors of the green and the blue faces are not parallel anymore.


You can try this if you connect two blocks that are meshed with only one cell. Then you can easily play around with the node coordinates.

#### negative volume

you've moved the mesh too far in one timestep so the cells have collapsed



## Cleaning terminal commands

# find and delete 
## remove all folders & files  except test2
find ./myfolder -mindepth 1 ! -regex '^./myfolder/test2\(/.*\)?' -delete
## remove only folders all except test2 
find ./myfolder -mindepth 1 -type d ! -regex '^./myfolder/test2\(/.*\)?' -delete
## remove only files all except test2  (only empty folders)
find ./myfolder -mindepth 1 -type f ! -regex '^./myfolder/test2\(/.*\)?' -delete
## remove only folders all except 0, constant, & system (all folders)
find . -mindepth 1 -maxdepth 1 -type d -not \( -name '0' -or -name 'constant' -or -name 'system' \) -exec rm -r {} \;
OR
find ./ -mindepth 1 -maxdepth 1 -type d ! -regex '^./\(0\|constant\|system\)?' -exec rm -r {} \;