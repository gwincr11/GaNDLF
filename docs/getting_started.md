This document will help you get started with GaNDLF using a few representative examples.

## Table of Contents

- [Table of Contents](#table-of-contents)
- [Installation](#installation)
- [Sample Data](#sample-data)
- [Segmentation](#segmentation)
  - [Segmentation using 3D Radiology Images](#segmentation-using-3d-radiology-images)
  - [Segmentation using 2D Histology Images](#segmentation-using-2d-histology-images)
- [Classification](#classification)
  - [Classification using 3D Radiology Images](#classification-using-3d-radiology-images)
  - [Classification (patch-level) using 2D Histology Images](#classification-patch-level-using-2d-histology-images)


## Installation

Please follow the [installation instructions](./setup.md) to install GaNDLF. This should end up with the shell that looks like this with the GaNDLF virtual environment activated:

```bash
(venv_gandlf) $> ### subsequent commands goes here
```

[Back To Top &uarr;](#table-of-contents)


## Sample Data

We will use the sample data used for our extensive automated unit tests for all examples. The data is available from [this link](https://upenn.box.com/shared/static/y8162xkq1zz5555ye3pwadry2m2e39bs.zip):

```bash
(venv_gandlf) $> wget https://upenn.box.com/shared/static/y8162xkq1zz5555ye3pwadry2m2e39bs.zip -O ./gandlf_sample_data.zip
(venv_gandlf) $> unzip ./gandlf_sample_data.zip
# this should extract a directory called `data` in the current directory
```
The contents of the `data` directory should look like this (for brevity, this locations shall be referred to as `${GANDLF_DATA}` in the rest of the document):

```bash
(venv_gandlf) $>  ls data
2d_histo_segmentation    2d_rad_segmentation    3d_rad_segmentation
# and a bunch of CSVs which can be ignored
```

**Note**: When using your own data, it is vital to correctly [prepare your data](https://mlcommons.github.io/GaNDLF/usage#preparing-the-data) prior to using it for any computational task (such as AI training or inference).

[Back To Top &uarr;](#table-of-contents)


## Segmentation
### Segmentation using 3D Radiology Images

1. Download and extract the [sample data](#sample-data) as described above.
2. [Construct the main data file](https://mlcommons.github.io/GaNDLF/usage#constructing-the-data-csv) that will be used for the entire computation cycle. For the sample data for this task, it should look like this:

    ```csv
    SubjectID,Channel_0,Label
    001,${GANDLF_DATA}/3d_rad_segmentation/001/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/001/mask.nii.gz
    002,${GANDLF_DATA}/3d_rad_segmentation/002/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/002/mask.nii.gz
    003,${GANDLF_DATA}/3d_rad_segmentation/003/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/003/mask.nii.gz
    004,${GANDLF_DATA}/3d_rad_segmentation/004/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/004/mask.nii.gz
    005,${GANDLF_DATA}/3d_rad_segmentation/005/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/005/mask.nii.gz
    006,${GANDLF_DATA}/3d_rad_segmentation/006/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/006/mask.nii.gz
    007,${GANDLF_DATA}/3d_rad_segmentation/007/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/007/mask.nii.gz
    008,${GANDLF_DATA}/3d_rad_segmentation/008/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/008/mask.nii.gz
    009,${GANDLF_DATA}/3d_rad_segmentation/009/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/009/mask.nii.gz
    010,${GANDLF_DATA}/3d_rad_segmentation/010/image.nii.gz,${GANDLF_DATA}/3d_rad_segmentation/010/mask.nii.gz
    ```
3. [Construct the configuration file](https://mlcommons.github.io/GaNDLF/usage#customize-the-training) that will help design the computation (training and inference) pipeline. An example file for this task can be found [here](https://github.com/mlcommons/GaNDLF/blob/master/samples/config_getting_started_segmentation_rad3d.yaml). This configuration has various levels of customization, and those details are [in this page](https://mlcommons.github.io/GaNDLF/customize.html).
4. Now you are ready to [train your model](https://mlcommons.github.io/GaNDLF/usage#running-gandlf-traininginference).
5. Once the model is trained, you can infer it on unseen data. Remember to construct a [similar data file](https://mlcommons.github.io/GaNDLF/usage#constructing-the-data-csv) for the unseen data, just without `Label` or `ValueToPredict` headers.

[Back To Top &uarr;](#table-of-contents)

### Segmentation using 2D Histology Images

1. Download and extract the [sample data](#sample-data) as described above.
2. [Extract patches/tiles from the full-size whole slide images](https://mlcommons.github.io/GaNDLF/usage#offline-patch-extraction-for-histology-images-only) for training. A sample configuration to extract patches is [here](https://github.com/mlcommons/GaNDLF/blob/master/samples/config_getting_started_segmentation_histo2d_patchExtraction.yaml).
3. [Construct the main data file](https://mlcommons.github.io/GaNDLF/usage#constructing-the-data-csv) that will be used for the entire computation cycle. For the sample data for this task, it should get generated after [the patches are extracted](https://mlcommons.github.io/GaNDLF/usage#offline-patch-extraction-for-histology-images-only), and should look like this:

    ```csv
    SubjectID,Channel_0,Label
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_720-3344.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_720-3344_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_816-3488.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_816-3488_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_960-3376.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_960-3376_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_976-3520.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_976-3520_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1024-3216.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_1024-3216_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1104-3360.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_1104-3360_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1168-3104.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_1168-3104_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1248-3248.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_1248-3248_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1312-3056.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_1312-3056_LM.png
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1392-3200.png,${GANDLF_DATA}/histo_patches_output/1/mask/mask_patch_1392-3200_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_720-3344.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_720-3344_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_816-3488.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_816-3488_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_960-3376.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_960-3376_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_976-3520.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_976-3520_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1024-3216.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_1024-3216_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1104-3360.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_1104-3360_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1168-3104.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_1168-3104_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1248-3248.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_1248-3248_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1312-3056.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_1312-3056_LM.png
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1392-3200.png,${GANDLF_DATA}/histo_patches_output/2/mask/mask_patch_1392-3200_LM.png
    ```
4. [Construct the configuration file](https://mlcommons.github.io/GaNDLF/usage#customize-the-training) that will help design the computation (training and inference) pipeline. An example file for this task can be found [here](https://github.com/mlcommons/GaNDLF/blob/master/samples/config_getting_started_segmentation_histo2d.yaml). This configuration has various levels of customization, and those details are [in this page](https://mlcommons.github.io/GaNDLF/customize.html).
5. Now you are ready to [train your model](https://mlcommons.github.io/GaNDLF/usage#running-gandlf-traininginference).
6. Once the model is trained, you can infer it on unseen data. Remember to construct a [similar data file](https://mlcommons.github.io/GaNDLF/usage#constructing-the-data-csv) for the unseen data, just without `Label` or `ValueToPredict` headers.

[Back To Top &uarr;](#table-of-contents)


## Classification
### Classification using 3D Radiology Images

1. Download and extract the [sample data](#sample-data) as described above.
2. [Construct the main data file](https://mlcommons.github.io/GaNDLF/usage#constructing-the-data-csv) that will be used for the entire computation cycle. For the sample data for this task, it should look like this (currently, the `Label` header is unused and ignored for classification/regression):

    ```csv
    SubjectID,Channel_0,ValueToPredict
    001,${GANDLF_DATA}/3d_rad_segmentation/001/image.nii.gz,0
    002,${GANDLF_DATA}/3d_rad_segmentation/002/image.nii.gz,1
    003,${GANDLF_DATA}/3d_rad_segmentation/003/image.nii.gz,0
    004,${GANDLF_DATA}/3d_rad_segmentation/004/image.nii.gz,2
    005,${GANDLF_DATA}/3d_rad_segmentation/005/image.nii.gz,0
    006,${GANDLF_DATA}/3d_rad_segmentation/006/image.nii.gz,1
    007,${GANDLF_DATA}/3d_rad_segmentation/007/image.nii.gz,0
    008,${GANDLF_DATA}/3d_rad_segmentation/008/image.nii.gz,2
    009,${GANDLF_DATA}/3d_rad_segmentation/009/image.nii.gz,0
    010,${GANDLF_DATA}/3d_rad_segmentation/010/image.nii.gz,1
    ```
3. [Construct the configuration file](https://mlcommons.github.io/GaNDLF/usage#customize-the-training) that will help design the computation (training and inference) pipeline. An example file for this task can be found [here](https://github.com/mlcommons/GaNDLF/blob/master/samples/config_getting_started_classification_histo3d.yaml). This configuration has various levels of customization, and those details are [in this page](https://mlcommons.github.io/GaNDLF/customize.html).
4. Now you are ready to [train your model](https://mlcommons.github.io/GaNDLF/usage#running-gandlf-traininginference).
5. Once the model is trained, you can infer it on unseen data. Remember to construct a [similar data file](https://mlcommons.github.io/GaNDLF/usage#constructing-the-data-csv) for the unseen data, just without `Label` or `ValueToPredict` headers.

[Back To Top &uarr;](#table-of-contents)

### Classification (patch-level) using 2D Histology Images

1. Download and extract the [sample data](#sample-data) as described above.
2. [Extract patches/tiles from the full-size whole slide images](https://mlcommons.github.io/GaNDLF/usage#offline-patch-extraction-for-histology-images-only) for training. A sample configuration to extract patches is [here](https://github.com/mlcommons/GaNDLF/blob/master/samples/config_getting_started_segmentation_histo2d_patchExtraction.yaml).
3. [Construct the main data file](https://mlcommons.github.io/GaNDLF/usage#constructing-the-data-csv) that will be used for the entire computation cycle. For the sample data for this task, it should get generated after [the patches are extracted](https://mlcommons.github.io/GaNDLF/usage#offline-patch-extraction-for-histology-images-only), and should look like this (currently, the `Label` header is unused and ignored for classification/regression):

    ```csv
    SubjectID,Channel_0,ValueToPredict
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_720-3344.png,0
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_816-3488.png,0
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_960-3376.png,0
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_976-3520.png,0
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1024-3216.png,1
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1104-3360.png,1
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1168-3104.png,1
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1248-3248.png,1
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1312-3056.png,1
    1,${GANDLF_DATA}/histo_patches_output/1/image/image_patch_1392-3200.png,1
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_720-3344.png,0
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_816-3488.png,0
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_960-3376.png,0
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_976-3520.png,0
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1024-3216.png,1
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1104-3360.png,1
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1168-3104.png,0
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1248-3248.png,1
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1312-3056.png,1
    2,${GANDLF_DATA}/histo_patches_output/2/image/image_patch_1392-3200.png,1
    ```
4. [Construct the configuration file](https://mlcommons.github.io/GaNDLF/usage#customize-the-training) that will help design the computation (training and inference) pipeline. An example file for this task can be found [here](https://github.com/mlcommons/GaNDLF/blob/master/samples/config_getting_started_classification_histo2d.yaml). This configuration has various levels of customization, and those details are [in this page](https://mlcommons.github.io/GaNDLF/customize.html).
5. Now you are ready to [train your model](https://mlcommons.github.io/GaNDLF/usage#running-gandlf-traininginference).
6. Once the model is trained, you can infer it on unseen data. Remember to construct a [similar data file](https://mlcommons.github.io/GaNDLF/usage#constructing-the-data-csv) for the unseen data, just without `Label` or `ValueToPredict` headers.

[Back To Top &uarr;](#table-of-contents)