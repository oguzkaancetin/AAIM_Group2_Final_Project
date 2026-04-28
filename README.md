# Convertible GAN Synthetic Data Project

This project trains a WGAN-GP model to generate synthetic Convertible vehicle images and evaluates whether the generated images improve downstream vehicle body-type classification.

The repository is centered around one end-to-end notebook:

- `Convertible_Project.ipynb`

The notebook contains:

- Convertible image preprocessing
- WGAN-GP training
- Synthetic image generation
- FID and Inception Score analysis
- Gradient curves and convergence analysis
- ResNet18 downstream classification experiment

## Project Structure

```text
.
|-- Convertible_Project.ipynb        # Main runnable notebook
|-- requirements.txt                 # Python dependencies
|-- README.md                        # Setup and run instructions
|-- Convertible/                     # Local real Convertible images, not committed
`-- convertible_outputs_wgan_gp_128x64/
    |-- checkpoints/                 # Generated checkpoints, not committed
    |-- samples/                     # Generated sample grids, not committed
    `-- synthetic_epoch_500/         # Generated synthetic images, not committed
```

Large generated files and local datasets are intentionally ignored by Git. The notebook explains how they are produced.

## Problem Statement

Vehicle body-type classifiers need diverse and balanced image data. Real vehicle images can be costly to collect and label, and some classes may be underrepresented. This project focuses on the Convertible class and asks:

> Can GAN-generated synthetic Convertible images improve real-image classification performance?

## Dataset

GAN training uses real Convertible images placed in:

```text
Convertible/
```

The current local dataset contains 811 Convertible images.

The downstream classifier uses the Kaggle dataset:

```text
ademboukhris/cars-body-type-cropped
```
It can be directly downloaded from following link: https://drive.google.com/drive/folders/1uTvPpgY2wWG_8544Ij8-5SVAHCXVLq2a?usp=sharing

The classmodel experiment compares:

- Baseline: 100 real Convertible images
- Augmented: 100 real Convertible images + 200 randomly selected synthetic Convertible images

The test set remains fully real.

## Environment Setup

Create and activate a Python environment, then install dependencies:

```bash
pip install -r requirements.txt
```

If you use Jupyter:

```bash
jupyter notebook
```

or:

```bash
jupyter lab
```

## Data Setup

The notebook expects real Convertible images in:

```text
Convertible/
```

This folder is used as the GAN training set. It is not included in the GitHub repository on purpose. The folder contains image data, and committing the full dataset would make the repository unnecessarily large. The project brief also asks for a clean repository without large binaries.

If this folder is missing, create it locally and place real Convertible training images inside it. A practical source is the Convertible class from the Kaggle Cars Body Type dataset:

```text
Cars_Body_Type/train/Convertible/
```

Copy or rename that folder into the project root as:

```text
Convertible/
```

Expected local structure:

```text
.
|-- Convertible_Project.ipynb
|-- README.md
|-- requirements.txt
`-- Convertible/
    |-- image_001.jpg
    |-- image_002.jpg
    `-- ...
```

The same rule applies to generated outputs such as checkpoints, generated samples, and synthetic image folders. They are produced by the notebook and are excluded from Git using `.gitignore`.

The downstream classification part downloads the Kaggle dataset through `kagglehub`.

## How to Run

Open:

```text
Convertible_Project.ipynb
```

Run the notebook from top to bottom.

Main stages:

1. Load real Convertible images.
2. Train WGAN-GP for 500 epochs.
3. Export 500 synthetic Convertible images.
4. Compute FID and Inception Score.
5. Plot gradient curves and convergence analysis.
6. Train ResNet18 baseline classifier.
7. Train ResNet18 augmented classifier.
8. Compare precision, recall, F1-score, and confusion matrix.

## Outputs

Generated outputs are written to:

```text
convertible_outputs_wgan_gp_128x64/
```

Important output files:

```text
convertible_outputs_wgan_gp_128x64/final_generated_grid.png
convertible_outputs_wgan_gp_128x64/training_curves_convergence.png
convertible_outputs_wgan_gp_128x64/synthetic_epoch_500/
convertible_outputs_wgan_gp_128x64/checkpoints/
```

These files are not committed because checkpoints and generated images can be large.
