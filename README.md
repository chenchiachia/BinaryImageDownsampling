# Topology-Preserving Binary Image Downsampling

## ECCV 2024: Topology-Preserving Downsampling of Binary Images
### Summary
We propose the first ***topology-preserving*** binary image downsampling method that:
1. works for ***large (512x512 and up) input images***,
2. generate ***high-quality results***,
3. can do high level downsampling (e.g., 8x, 16x) out-of-the-box, and
4. is ***resonably*** fast (<1 seconds)
### What are binary images and why they are important?
Binary images are 2D images consisting of exactly two colors. They encode many things in AI such as ***segmentation masks*** in CV and ***2D maps*** in computer games. 
<div style="align: center">
  <img src="https://github.com/chenchiachia/BinaryImageDownsampling/blob/main/figures/binary_images.png?raw=true"/>
</div>

### What are the benifits of using smaller, downsampled binary images?
**1. Smaller computational costs:**
> Binary imageoperations are time-critical and some are actually expensive, such as ***persistent homology (PH) computation*** of 2D masks and ***shortest path computations*** in 2D masks.

**2. Better visualization:**
> After downsampling, small details become larger and more legible (if not getting erased!)

<div style="align: center">
  <img src="https://github.com/chenchiachia/BinaryImageDownsampling/blob/main/figures/benefit.png?raw=true"/>
</div>

### Why not just using existing downsampling methods?
All exisiting methods are likely to alter the ***topology*** of binary images - e.g., different components merged, small spots and holes erased, etc.

<div style="align: center">
  <img src="https://github.com/chenchiachia/BinaryImageDownsampling/blob/main/figures/exisisting_method_problem.png?raw=true"/>
</div>

### Our integer programming optimization-based approach
We formulate the problem as an optimization problem with Boolean variables only. Our main contribution is beging able to ***model the topology-preserving constraints as a list of linear constraints***.

<div style="align: right">
  <img src="https://github.com/chenchiachia/BinaryImageDownsampling/blob/main/figures/integer_programming.png?raw=true"/>
</div>

We ensure every component-component boundary to ***remain a closed loop*** after downsampling.

### Quantitative results
We tested on following two datasets:
1. CNCB CT masks taken from COVID-positive patients(http://ncov-ai.big.ac.cn/download, ct_lesion_seg.zip):542 binary images
2. Aerial roof masks (https://humansintheloop.org/resources/datasets/semantic-segmentation-dataset-2/): 255(512x512) images, randomly sampled from a larger satellite imagery collection.

Our method generated ***100% topologically correct results*** while still having roughly ***the same similarity scores*** (IoU and Dice) as the best performing existing methods.



## SIGGRAPH Asia 2024 Poster: Shortest Path Speed-up Through Binary Image Downsampling
## How to use topology-preserving downsampling tool

1. Open the "downsampling" project, and select the "x64" platform and "Release" configurtion.
2. Build the project, and the "downsampling,exe" excutable will be generated in the "x64/Release" folder.
3. Run the following command to downsample an image.

   ```
   downsampling.exe <image_filename> [<bigpixel_width> <bigpixel_height> <[land_weight> [<calculate_errors> [<save_components> [<neighborhood_offset>]]]]]
   ```
   ### Parameter Discription
   - <input_filename>: Path to the input binary image file (required)
   - <bigpixel_width> and <bigpixel_height>: Width and height of a bigpixel (optional, integer)
     - downsampling factor
     - Default value: 4x4
   - <land_weight>: Land (Foreground) weight (optional, integer)
     - Default value: 2
   - <calculate_errors>: Whether to calculate errors (opptional, 0 or 1)
     - Default value: 0 (do not calculate)
   - <save_components>: Whether to output component index map (optional, 0 or 1)
     - Default value: 0 (do not output)
   - <neighborhood_offset>: Coverage offset
      - Default: 0

<div style="align: center">
  <img src="https://github.com/chenchiachia/BinaryImageDownsampling/blob/main/figures/downsampling_example.png?raw=true"/>
</div>

## How to use dilation downsampling tool

1. To downsample a single image, run the following command:
   ```
   python dilation.py <image_filename> [output_path.jpg]
   ```
2. To process all images in a directory, use the following command:
    ```
   python dilation.py input_directory [output_directory]
   ```

