
# 16.04.2025 - SESAME SUNSTONE Training Programme - First Edition
## Make the best of your Synchrotron Tomography Experiment - 3D Image Analysis Crash Course

| Speaker ||
| :--- | --- |
| **Dr.-Ing. Gianluca Iori** |
| Research Assistant in Advanced X-ray Imaging, Institute for Biomedical Engineering, ETH Zurich, Switzerland |
| BEATS beamline scientist, Synchrotron-light for Experimental Science and Applications in the Middle East - SESAME, Jordan |

## Contents
1. [Introduction to 3D image processing software](#part-1-introduction-to-3d-image-processing-software)
2. [ImageJ basic operations](#part-2-imagej-basic-operations)
3. [3D image processing with Dragonfly](#part-3-image-processing-with-dragonfly)
4. [XCT 3D image processing hands-on session](#part-4-xct-3d-image-processing-hands-on-session)

---
## Resources
| Software ||
| :--- | --- |
| [Fiji (is just ImageJ)](https://fiji.sc/) | Image processing package. |
| [ORS Dragonfly](https://dragonfly.comet.tech/) | 3D image visualization and analysis software. |
| [ParaView](https://www.paraview.org/) | An open-source, multi-platform data analysis and visualization application. |
| [3DSlicer](https://www.slicer.org/) | Open source software platform for medical image informatics, image processing, and three-dimensional visualization. |
| [Silx](https://www.silx.org/doc/silx/latest/install.html) | Inspect your beamtime RAW data (sinograms). |

---
## References
See the file ...

---
## Open dataset
Zenodo link ...

### Part 1: Introduction to 3D image processing software
- Outline of this lecture
- [Software resources](resources)
- Inspect beamtime RAW data and metadata using [Silx](https://www.silx.org/doc/silx/latest/install.html)

---
### Part 2: ImageJ basic operations
- Loading large dataset in ImageJ as virtual stacks
    - Stacks are 3d volumes represented as a collection of (`.tiff`) slices
- Image/Show Info
- Show scale bar
- Adjust pixel size (Image/Properties)
- Histogram control 
- Adjust contrast
- Set threshold
- Draw line; Analyze/Measure
    - Line profile
- 3D filtering (Process/Filters/Gaussian Blur)
    - Check once again how a line profile looks like
- Crop 2D
- Rescale with factor 4 (Image/Scale)
- 3D ROI crop (Plugins/Stacks/Crop (3D))
- 8-bit conversion
- Save! (File/Save As/Image Sequence)

---
### Part 3: Image processing with Dragonfly
#### Load and display 3D data
- Load images
- Window leveling controls
- 2D views orientation
- 3D view settings and window leveling
- Slab thickness, min and max projection
- Export screenshots
- Scale and colorbar

#### Stitching of 3D images
- Load 2 image stacks
- Crop
- Move dataset
- 3D stitch

#### Porosity analysis - part 1
- Median filter
- Crop
- Foreground mask
- Whole sample mask
- Voids mask
- Calculate porosity

#### Porosity analysis - part 2
- Connectivity analysis and multi-ROI
- Label pores by size
- Remove pores below 4 pixels
- Compare pore size distributions of different materials
- Export results
- Color pore by size, aspect ratio…
- 3D view hiding part of the sample
- Largest interconnected region
- Thickness map

#### Thresholding
- [X] Manual 3D ROI selection in 3DSlicer: separate VFA and MBA
    - File/Add Data
    - **Segment Editor module**
        - Add
        - **Draw**
        - Fill between slices/Initialize/Allow overlap/Apply
        - Export as STL
            - Segmentations button
            - Export to new labelmap
    - **Segmentation module**
        - Save
- [X] Inspect it in ImageJ
    - A mask is made of ones and zeroes
    - Image/Overlay/Add Image
- [X] Automatic thresholding in ImageJ: bone_tissue mask
    - Load (Virtual stack off)
    - Image/Adjust/Threshold
        - Otsu, Auto -> Apply
- [X] Remove noise speckles
    - Process/Noise/Despeckle

#### Perform checks!!
We check that the masking operation was fine, that we haven't introduced artefacts (e.g. wrongly removed bone areas).
- [X] Image/Color/Merge Channels
    - `C2 (green): MBA_tissue`
    - `C4 (gray): cropped_rescale_025`
![Overlay](figures/Composite.png)

#### Morphological operations (mask refinement)
- [X] Keep largest strut (remove unconnected objects)
    - [Plugins/MorphoLibJ/Binary Images/Keep Largest Region](https://imagej.net/MorphoLibJ)
    - [Plugins/BoneJ/Purify](https://bonej.org/purify) (alternatively)
    - Image/Rename (bone_tissue)
    - File/Save As
- [X] (Alternatively) Image open (remove unconnected objects)
    - Process/Binary/Open
- [X] Logical operations on masks: VFA_tissue; MBA_tissue
    - Load MBA-VFA separation
    - Process/Binary/Make binary
    - Process/Image Calculator: `bone_tissue AND VFA_separation`
    - Plugins/MorphoLibJ/Binary Images/Keep Largest Region    
    - Image/Rename (VFA_tissue)
    - File/Save As
    - Process/Image Calculator: `bone_tissue - VFA_tissue`
    - Image/Rename (MBA_tissue)
    - File/Save As
#### Towards Bone Volume Fraction (BV/TV)!!
- [X] Image Erode, Dilate, Open, Close: MBA_total and VFA_total
    - Plugins/MorphoLibJ/Morphological Filters (3D)
        - Dilate; Cube (4x4x4)
    - Plugins/MorphoLibJ/Extend Image Borders (L1, R1, T0, B1, F1, B1; White)
    - Plugins/3D/3D Fill Holes
    - Plugins/Stacks/Crop 3D [1:649, 0:648, 2:350]
    - Plugins/MorphoLibJ/Binary Images/Keep Largest Region    
    - Image/Rename (MBA_total)
    - File/Save As
    - Plugins/MorphoLibJ/Morphological Filters (3D)
        - Erosion; Cube (4x4x4)
- [X] MBA_pores and VFA_pores
    - Edit/Invert `bone_tissue`
    - Process/Image Calculator: `bone_tissue AND MBA_total_eroded`
    - Process/Noise/Despeckle (necessary?)
    - Image/Rename (MBA_pores)
    - File/Save As
    
#### Quantitative analysis (ImageJ-BoneJ)
- [X] BV/TV
    - Load MBA_tissue, MBA_total, VFA_tissue, VFA_total
    - Plugins/BoneJ/Fraction/Volume Fraction
    - Repeat for MBA_tissue, MBA_total, VFA_tissue, VFA_total; we are only interested in TV
    - `BVTV_MBA = TV_MBA_tissue / TV_MBA_total`
    - `BVTV_VFA = TV_VFA_tissue / TV_VFA_total` 
- [X] Tissue density
    - Process/Image Calculator: `MBA_tissue AND cropped_rescale_025`
    - Histogram `(shortcut: "h")`
    - Repeat for VFA
- [ ] Connectivity
- [ ] Thickness
    - Pores
    - Trabeculae
- [ ] Skeleton Analyzer
![Skeleton Analyzer](figures/Skeleton.png)
- [ ] Particle Analyzer

#### Computed Tomography to Finite Elements
[CT2FE](https://github.com/gianthk/CT2FE) - From 3D CT datasets to voxel-Finite Element (FE) models for the prediction of bone tissue stiffness.

#### 3D Visualization (Paraview)
- [X] Slice
- [X] Threshold
- [X] Clip
![Paraview visualization](figures/MBA-VFA_pores_Paraview.png)
