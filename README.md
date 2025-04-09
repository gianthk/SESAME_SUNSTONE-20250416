
# 16.04.2025 - SESAME SUNSTONE Training Programme - First Edition
## Make the best of your Synchrotron Tomography Experiment - 3D Image Analysis Crash Course


[![DOI](https://zenodo.org/badge/796287736.svg)](https://doi.org/10.5281/zenodo.14592120)


| Speaker |
| :--- |
| **Dr.-Ing. Gianluca Iori** |
| Research Assistant in Advanced X-ray Imaging, Institute for Biomedical Engineering, ETH Zurich, Switzerland |
| BEATS beamline scientist, Synchrotron-light for Experimental Science and Applications in the Middle East - SESAME, Jordan |

## Contents
1. [Introduction to 3D image processing software](#part-1-introduction-to-3d-image-processing-software)
2. [ImageJ basic operations](#part-2-imagej-basic-operations)
3. [3D image processing with Dragonfly](#part-3-image-processing-with-dragonfly)
4. [Image segmentation](#part-4-image-segmentation)
5. [Pore analysis](#part-5-pore-analysis)

---
## Resources
| Software ||
| :--- | --- |
| [Fiji (is just ImageJ)](https://fiji.sc/) | Image processing package. |
| [ORS Dragonfly](https://dragonfly.comet.tech/) | 3D image visualization and analysis software. |
| [ParaView](https://www.paraview.org/) | An open-source, multi-platform data analysis and visualization application. |
| [3DSlicer](https://www.slicer.org/) | Open source software platform for medical image informatics, image processing, and three-dimensional visualization. |
| [Silx](https://www.silx.org/doc/silx/latest/install.html) | Inspect your beamtime RAW data (sinograms). |
| [Biomedisa](https://biomedisa.info/) | Open-source application for segmenting large 3D image data. |

---
## References
For a list of works on the application of synchrotron Computed Tomography in cultural herigage science see the [bibliography file](cultural_heritage.bib).

---
## Open dataset
### Synchrotron X-ray Computed Tomography images from the ID10-BEATS beamline of SESAME
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.15182529.svg)](https://doi.org/10.5281/zenodo.15182529)

Contents:
- `wasp_recon_small.zip` - SXCT reconstruction of scan of a wasp performed at beamline ID10-BEATS of SESAME
- `figurine_e1_macro-20241001T114706_crop_align.zip` - SXCT reconstruction of scan of historical terracotta at beamline ID10-BEATS of SESAME

---
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
    - Process/Noise/Despeckle
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

---
### Part 4: Image segmentation
- Manual 3D ROI painting
- Select range; Otsu thresholding
- A mask is made of ones and zeroes
- Otsu thresholding in ImageJ
- Image/Adjust/Threshold
    - Otsu, Auto -> Apply
    - Image/Overlay/Add Image
- Dragnfly Image plugins -> K-means segmentation
- Dragonfly tutorial: deep learning segmentation
- [Biomedisa](https://biomedisa.info/)
- [Segment anything](https://segment-anything.com/)

#### Morphological operations (mask refinement)
- Keep largest strut (remove unconnected objects)
    - [Plugins/MorphoLibJ/Binary Images/Keep Largest Region](https://imagej.net/MorphoLibJ)
    - [Plugins/BoneJ/Purify](https://bonej.org/purify) (alternatively)
- (Alternatively) Image open (remove unconnected objects)
    - Process/Binary/Open
- Logical operations on masks
    - Process/Image Calculator: AND, OR, ...
- Morphological operators
    - Image Erode, Dilate, Open, Close
    - Plugins/MorphoLibJ/Morphological Filters (3D)
        - Dilate; Cube (4x4x4)
    - Plugins/3D/3D Fill Holes
    - Process/Noise/Despeckle
    - Edit/Invert

### Part 5: Pore analysis

#### Porosity analysis
- Median filter
- Crop
- Foreground mask
- Whole sample mask
- Voids mask
- Calculate porosity

#### Pore size analysis
- Connectivity analysis and multi-ROI
- Label pores by size
- Remove pores below 4 pixels
- Compare pore size distributions of different materials
- Export results
- Color pore by size, aspect ratio…
- 3D view hiding part of the sample
- Largest interconnected region
- Thickness map





    
