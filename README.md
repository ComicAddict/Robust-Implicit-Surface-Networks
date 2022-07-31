# Robust Computation of Implicit Surface Networks for Piecewise Linear Functions

![](https://user-images.githubusercontent.com/8947527/182003910-f1e39f30-e67b-498b-b990-c2f5964d5020.jpg)

[Xingyi Du](https://duxingyi-charles.github.io/), [Qingnan Zhou](https://research.adobe.com/person/qingnan-zhou/),  [Nathan Carr](https://research.adobe.com/person/nathan-carr/), [Tao Ju](https://www.cse.wustl.edu/~taoju/)
<br/>*ACM Transaction on Graphics (Proceedings of SIGGRAPH 2022)*<br/>

[`Project Page`](https://duxingyi-charles.github.io/publication/robust-computation-of-implicit-surface-networks-for-piecewise-linear-functions/)

## Abstract

Implicit surface networks, such as arrangements of implicit surfaces and materials interfaces, are used for modeling piecewise smooth or partitioned shapes. However, accurate and numerically robust algorithms for discretizing either structure on a grid are still lacking. We present a unified approach for computing both types of surface networks for piecewise linear functions defined on a tetrahedral grid. Both algorithms are guaranteed to produce a correct combinatorial structure for any number of functions. Our main contribution is an exact and efficient method for partitioning a tetrahedron using the level sets of linear functions defined by barycentric interpolation. To further improve performance, we designed look-up tables to speed up processing of tetrahedra involving few functions and introduced an efficient algorithm for identifying nested 3D regions.

## Code

- impl_arrangement, a command line program that discretizes the arrangements of the 0-level sets of a collection of functions on a tetrahedron grid. 
- material_interface, a command line program that discretizes the material interfaces of a collection of material functions on a tetrahedron grid.

The programs have been tested on macOS 12.4.

## Build

### Mac

    mkdir build
    cd build
    cmake -DCMAKE_BUILD_TYPE=Release ..
    make

The program `impl_arrangement` and `material_interface` will be generated in the `build` subdirectory.

## Usage

### impl_arrangement

This program takes a tetrahedronal grid and a collection of functions, and discretizes the implicit arrangements of the functions on the grid.

Usage: 

    ./impl_arrangement [OPTIONS] config_file

Positionals:
- config_file (REQUIRED): Configuration file, specifying input/output paths and algorithm parameters.

Options:
- -h,--help : Print help message and exit.
- -T,--timing-only BOOLEAN    Record timing without output result. Default is False.
- -R,--robust-test BOOLEAN    Perform robustness test. Default is False.

The `config_file` should be a JSON file with the following named parameters:
- `tetMeshFile`: Path to the file storing input tetrahedral grid. It is a JSON file storing vertex coordinates and tets. See `example/tet_mesh/tet5_grid_10k.json` for an example.
- `funcFile`: Path to the file storing input implicit functions.
- `outputDir`: Directory to store output files.
- `useLookup`: Whether to use look-up tables to speed-up tet processing. Default value is "true".
- `use2funcLookup`: Whether to use look-up tables for tetrahedral with two active functions. Default value is "true".
- `useTopoRayShooting`: Whether to use topological ray shooting to resolve the nesting order of multiple connected components of arrangement mesh (see section x of our paper). Default value is "true".

Note that `tetMeshFile`, `funcFile` and `outputDir` take absolute path.

An example config file is `example\implicit_arrangement\config.json`. You should change the paths in the config file according to your own system. 

Test it by:

    ./impl_arrangement example/implicit_arrangement/config.json


### material_interface

Usage: 

    ./material_interface [OPTIONS] config_file

Positionals:
- config_file (REQUIRED): Configuration file, specifying input/output paths and algorithm parameters.
Options:
- -h,--help : Print help message and exit.
- -T,--timing-only BOOLEAN    Record timing without output result
- -R,--robust-test BOOLEAN    Perform robustness test.
