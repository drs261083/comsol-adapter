# preCICE-adapter for the FEM code COMSOL Multi-Physics

This folder contains files to use COMSOL as the structural solver in an FSI simulation.
Since it was not possible to directly couple the C++ coded preCICE to the COMSOL
API, we used FSI\*ce as intermediate communication/data library. There are, hence,
two important files: `comsol_simulation.c`, the interface with the COMSOL API, which uses
FSI\*ce to communicate the data to the intermediate (executable) component 
`ComsolPrecice.c`. `ComsolPrecice` uses FSI\*ce to exchange data with 
`comsol_simulation`, and preCICE for the actual coupling.

Only 2D simulations have been done so far. For 3D, several changes to 
`comsol_simulation.c` and some to `ComsolPrecice.c` have to be done. 



## How to compile the files?

The compilation is done by SCons, check the SConstruct script for detailed 
information on the build variables. There is a debug and a release mode.

`comsol_simulation.c` is compiled and linked into a shared object (.so). For that,
it needs (among others) the pathes to the COMSOL installation, and the FSI\*ce
libraries. These paths are hardcoded in the SConstruct file.

`ComsolPrecice.c` is compiled into an executable. For that, it needs (among others)
the pathes to the FSI\*ce libraries and the preCICE library. 



## How to run COMSOL for FSI simulations?

There are two executables to be run (in any order). The Comsol script needs to be 
run as follows

$ comsol script simulation

The keyword "simulation" tells the COMSOL script to run the simulation.m file in the
current directory. It can also be omitted and given in the scripting GUI. To run
COMSOL script without GUI, type

$ comsol script -term

Wait until comsol started, then type 

$> flreport('off')
$> simulation

To run `ComsolPrecice`, type

> ./ComsolPrecice Comsol precice_config.xml

Where COMSOL gives the name of the participant to preCICE, and precice_config.xml
tells preCICE where to find the coupling configuration.

HINT: Sometimes, after killing the COMSOL script, it happens that COMSOL starts, but
      doesn't load the simulation.m file. It suffices to close and restart it.
