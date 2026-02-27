---
title: 'CellSculptor: A 3D Volume Generator for Sample-Informed Biomaterial Photomodulation'
tags:
  - biomaterials
authors:
  - name: Ryan M. Francis
    orcid: 0000-0002-3964-6090
    affiliation: 1
  - name: Ryan P. Brady
    orcid: 0000-0002-1338-1506
    affiliation: 1
  - name: Nels Schimek
    orcid: 0000-0003-2281-4075
    affiliation: 2
  - name: Cole A. DeForest
    orcid: 0000-0003-0337-3577
    affiliation: "1-6"
affiliations:
 - name: Department of Chemical Engineering, University of Washington, Seattle, USA
   index: 1
 - name: Department of Chemistry, University of Washington, Seattle, USA
   index: 2
 - name: Department of Bioengineering, University of Washington, Seattle, USA
   index: 3
 - name: Institute for Stem Cell & Regenerative Medicine, University of Washington, Seattle, USA
   index: 4
 - name: Molecular Engineering & Sciences Institute, University of Washington, Seattle, USA
   index: 5
 - name: Institute for Protein Design, University of Washington, Seattle, USA
   index: 6
date: 1 June 2025
bibliography: paper.bib
---

# Summary

Modern “image-guided” optical lithography enables fabrication and postsynthetic photocustomization of biomaterials with spatiotemporal complexity that mimics living tissue. These methods enable material patterning following top-down imposition of user-defined geometries through a variety of photochemical reactions. Though powerful, prior efforts utilize input patterning shapes that are generated independently from and ununiformed with respect to the sample onto which they are to be superimposed. CellSculptor is an open-source Python package addressing this limitationvia “sample-informed” generation of complex 3D digital photomasks based explicitly on features of the target volume (e.g., geometric arrangement of individual cells or assemblies). This software will facilitate advances in biological understanding and regenerative medicine by allowing cells and assemblies to be photochemically addressed as discrete 3D individuals rather than part of a homogenous bulk material.

# Statement of need

In the field of user-defined biomaterials, “image-guided” patterning, or the superposition of complex and variable patterns onto biological samples using photolithography, is an emerging field with exciting implications for many biological applications.1,2 Engineered materials with properties mimicking that of native tissue are invaluable assets for disease modeling and studying 3D biological systems. In a typical image-guided patterning workflow,  a computer-controlled light source transduces complex digital geometries to spatiotemporally direct photochemical processes in the sample. These geometries are infinitely customizable and range from arbitrary (e.g., artwork, photographs, 3D shapes) to biomimetic (e.g., spatial protein expression in a biopsy or 1:1 reproductions of imaged microvasculature).3–5 However, they are ultimately uninformed with respect to the original sample volume, which may itself be complex. Biomaterial samples often contain cells, organoids, or other features arranged randomly or semi-randomly throughout. For example, patterned microvasculature can hypothetically be placed at any location within a material, but it is only useful if cells are positioned to interact with it. Similarly, one of the promises of high-resolution 3D patterning techniques (e.g., multiphoton lithography and volumetric printing) is to dynamically address encapsulated cells as individuals, assemblies, or clusters rather than a homogenous whole. As such, it would be useful to build photomasks that only interact with a specific cell type in a tissue or 3D coculture sample or that are smaller than their targets (e.g., driving asymmetric polarization within a collection of organoids). This only works if the geometric arrangement of those sample-comprising cells has been communicated to the hardware. This has recently been attempted by coopting a commercial 3D modeling software (Rhino3D) in the context of volumetric printing, but applications were limited because the software is not open source and is not specialized for biomaterial patterning.6 An open-source toolbox is needed for broader uptake and future development of “sample-informed” volume generation in biomaterials lab. CellSculptor is a Python-based suite that constructs such 3D photomasks using an input image stack representing the target volume. This software is an important step toward fabricating biomaterials both complex and cell-optimized geometric features. 


# Capabilities

CellSculptor uses an input image stack representing the sample volume as a template for generating the output mask. In version 1, .tif z-stacks are used as both input and output. Support for other file types could be added in future versions depending on interest, but .tif is convenient because of its ubiquity in biological imaging and the relative ease with which grayscale intensity values can be encoded in 3D space. 

After extracting relevant metadata, the location and geometry of objects contained in the input is calculated. These objects may represent cells, organoids, assembloids, or other geometric features present in the sample depending on thresholding and centroid identification parameters. Once centroid coordinates are identified, they are placed on a 3D matrix, and user-specifed volumes are generated around them. Finally, these volumes are output as a 3D photomask (.tif z-stack) for subsequent image-guided photolithography (Fig. 1).

Single object encapsulation 

Object encapsulation is the simplest application of CellSculptor, surrounding individual cells or assemblies by 3D volumes of arbitrary complexity. A range of common 3D shapes (e.g., spheres, cylinders, cones, prisms) are hard-coded as ShapeClasses to facilitate rapid iteration. This provides a simplified workflow for applications that involve, for example, selectively stiffening hydrogel regions surrounding individual cells or activating biochemical cues in their immediate vicinity in comparatively simple 3D geometries. Importantly, CellSculptor masks should be applicable across scales as voxels are not size constrained in the physical world. This is critical for biomaterial fabrication since a variety of technologies accomplish similar goals across a broad range of resolutions (sub-µm to multi-cm) and volumetric throughputs (µL hr-1 to mL s-1).7 

Enabling creation of arbitrarily complex and user-defined volumes, CellSculptor accepts .stl files as an input. Examples of both simple (e.g., sphere, cone) and complex (e.g., Voronoi shell) photomasks overlaid on their respective input volumes are shown in Fig. 2. In each case, the volumes are centered over individual cells in the volume to be patterned. Properties of the 3D volumes including radius, aspect ratio, and rotation angle can be tuned as parameters by the user.  

Engineered complex gradients (i.e., “grayscale” patterning) are another recent advance in biomaterial photolithography, using dose-dependent and spatially varied photochemistry uniquely define extent of photocustomization at each voxel throughout the 3D sample. Although hardware requirements have restricted this type of patterning to only a few early examples,4,5,8 we felt it important to include a gradient module in CellSculptor. Radial- and function-based (directional) gradients are currently available, but more user-defined options may be added in later versions. Whereas function-defined gradients can be generated by simple matrix multiplication, the computational cost of overlaying arbitrarily complex 3D grayscale masks many times within a single patterning volume is incompatible with the current implementation. 

Object networking  

In addition to volume generation at each centroid in the input volume, CellSculptor also features a module for object networking in three different modes. 3D volumes are placed at the midpoint of each centroid pair and extended to its members. In aggregate, this can create both sparse and highly interconnected networks. Using “n_nearest”, the user defines how many such volumes extend from each centroid to its nearest neighbors. Alternatively, the “tsp” option uses a greedy algorithm to solve a modified traveling salesman problem where each node is visited n times.9 Finally, “custom” requires the user to submit a manually curated set of centroid pairs. We hope that this feature will be useful for examining and engineering multicellular behaviors such as metabolic coupling, intercellular communication, and neural circuits. 

 
# Competing Interests 

The authors declare no competing interests. 

# Code and Data Availability 

The source code, Jupyter Notebook, and all input image examples shown in this paper are available on GitHub (https://github.com/DeForestLab/CellSculptor). A public Jupyter Notebook is hosted through Google Colab (). Additionally, the cellsculptor package is distributed through __ and can be installed by __.  
 
# Acknowledgements 

This work was supported by a Maximizing Investigators’ Research Award (R35GM138036 to C.A.D.), a graduate training grant, (TL1TR002318 to R.P.B.) and research grants (R01CA289291 to C.A.D., R21CA283686 to C.A.D.) from the National Institutes of Health. 

# References