[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.209-blue.svg)](https://doi.org/10.25663/brainlife.app.209)

# app-multi-lap

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Segments white matter bundles in a target tractogram by solving multiple Linear Assignment Problems (multi-LAP) against example bundles from other subjects, with optional SLR registration, and outputs a WMC classification.**

Supervised, example-based bundle segmentation: for each bundle of interest it takes the corresponding bundles of several example subjects, optionally registers their tractograms to the target with streamline-based linear registration (SLR), and assigns target streamlines by solving linear assignment problems in a dissimilarity space. Use it to extract named bundles (e.g. AFQ or WMA tracts) in a new subject when segmented examples exist. It needs the target tractogram and T1, plus, for each example subject, its tractogram, T1 and WMC segmentation in the same order and with matching subject IDs.

| | |
|---|---|
| 📥 **Inputs** | `neuro/track/tck` ×2 · `neuro/anat/t1w` ×2 · `neuro/wmc` |
| 📤 **Outputs** | `neuro/wmc` |
| 🐳 **Container** | `brainlife/dipy:0.16.0` |
| ⚙️ **Resources** | 16 CPUs · 31 GB memory · 10 h walltime |
| 🔗 **Run it** | [doi.org/10.25663/brainlife.app.209](https://doi.org/10.25663/brainlife.app.209) |

#### 📥 Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `0` | `neuro/track/tck` | yes | Tractogram (.tck) of the target subject to segment. |
| `1` | `neuro/anat/t1w` (acpc_aligned) | yes | ACPC-aligned T1 of the target subject, used to convert its tractogram to .trk. |
| `2` | `neuro/wmc` | yes | WMC segmentations of the example subjects (multi), derived from their example tractograms. One per example subject, in the same order as the example tractograms and T1s; subject IDs must match. |
| `3` | `neuro/track/tck` | yes | Tractograms (.tck) of the example subjects (multi). |
| `4` | `neuro/anat/t1w` (acpc_aligned) | yes | ACPC-aligned T1s of the example subjects (multi). |

#### 📤 Outputs

| Output | Datatype | Description |
|---|---|---|
| `output_wmc` | `neuro/wmc` | WMC classification of the target tractogram with the segmented bundles of interest. |

<details>
<summary><b>⚙️ Parameters</b> (4)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `k` | number | `2000` | value for k-NN to compute the superset |
| `step_size` | number | `0.625` | step size for resampling |
| `slr` | boolean | `true` | If false, assuming subjects already co-registered in the same space |
| `tractID_list` | string |  | ID list of the bundles of interest, separated by commas |

</details>

📚 **References used by this App:**
- Sharmin N, Olivetti E, Avesani P. White Matter Tract Segmentation as Multiple Linear Assignment Problems. Front Neurosci (2018). [doi:10.3389/fnins.2017.00754](https://doi.org/10.3389/fnins.2017.00754)
- Avesani P, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

<!-- brainlife-app:end -->

This App segments white matter bundles by solving multiple Linear Assignment Problems (LAP or multi-LAP). The method is a supervised example-based segmentation method, and thus requires multiple bundles as examples to learn from. The segmentation is performed by means of fiber correspondence across subjects by considering the shape of the fibers (i.e. by computing fiber distances).

![](lap-original.png)

### Authors
- Giulia Bertò (giulia.berto.4@gmail.com)

### Contributors
- Emanuele Olivetti (olivetti@fbk.eu)

### Funding Acknowledgement
We kindly ask that you acknowledge the funding below in your publications and code reusing this code. \
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-AOC-1916518](https://img.shields.io/badge/NSF_AOC-1916518-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1916518)

### Citation
We kindly ask that you cite the following article when publishing papers and code using this code: \
["White Matter Tract Segmentation as Multiple Linear Assignment Problems"](https://doi.org/10.3389/fnins.2017.00754), Sharmin N., Olivetti E. and Avesani P. (2018) White Matter Tract Segmentation as Multiple Linear
Assignment Problems. Front. Neurosci. 11:754. doi: 10.3389/fnins.2017.00754

## Running the app
### On [Brainlife.io](http://brainlife.io/) 
You can submit this App online at https://doi.org/10.25663/brainlife.app.209 via the “Execute” tab.

Inputs: \
To perform the bundle segmentation, you need two key elements: (i) the tractogram of the (target) subject you want to extract the bundle from and (ii) the wmc segmentations of multiple (example) subjects you want to learn from. Moreover, you have to provide the anatomical T1s and the tractograms of the (example) subjects (which are used to apply an initial Streamline Linear Registration (SLR) between tractograms). The wmc segmentation files you have to provide as examples should be obtained using the AFQ algorithm (https://doi.org/10.25663/bl.app.13) or the the WMA algorithm (https://doi.org/10.25663/bl.app.41). You can choose the bundle to be segmented (one at the time) by providing the id related to the bundle to be segmented, from 1 to 20 if providing AFQ segmentations, and from 1 to 78 if providing WMA segmentations. 

Output: \
You will get the wmc segmentation of the bundle(s) of interest in the target subject.

#### Branch 1.0:
It also performs Nearest Neighbor (NN, or multi-NN) segmentation for comparison. This branch is associated with the App https://doi.org/10.25663/brainlife.app.174 (deprecated).

#### Branch 2.0:
As branch 2.0, but with trk input (deprecated).

#### Branch 3.0:
This branch implements the same functionalities of the branch 1.0 but with the following changes: 
- uses the new wmc input 
- SLR registration is optional
- can run multiple bundles at the same time 

You may provide as examples AFQ segmentations obtained by the app https://doi.org/10.25663/brainlife.app.207 or WMA segmentations obtained by the app https://doi.org/10.25663/brainlife.app.188.

#### Branch 3.1:
As branch 3.0 with some small enhancements.

### Running locally
1. git clone this repo.
2. Inside the cloned directory, create `config.json` with something like the following content with paths to your input files:
```
{
    "tractogram_static": "./track.tck",
    "t1_static": "./t1.nii.gz",
    "segmentations": [
        "./sub-1/classification.mat",           
        "./sub-2/classification.mat"
    ],
    "tracts": [
        "./sub-1/tracts",
        "./sub-2/tracts"
    ],
    "tractograms_moving": [
        "./sub-1/track.tck",
        "./sub-2/track.tck"
    ],
    "t1s_moving": [
        "./sub-1/t1.nii.gz",
        "./sub-2/t1.nii.gz"
    ],
    "tractID_list": "11, 12, 19, 20"
}
```
3. Launch the App by executing `main`.
```
./main
```

#### Dependencies
This App only requires [singularity](https://sylabs.io/singularity/) to run.

#### MIT Copyright (c) 2020 Bruno Kessler Foundation (FBK)
