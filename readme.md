# Weld-VTK Dataset

This repository contains raw data for visual perception and gaze studies on welding images. The README below is organized around three main directories in the repository:

- `description/`: textual descriptions or metadata for each image (example: `001_1_768_1536_description.txt`).
- `gaze/`: gaze recordings in CSV format that correspond to images (example: `001_1_768_1536.csv`).
- `images/`: original or cropped images (common extensions: `.jpg`, `.png`, etc.).

Repository URL:

```
https://github.com/wangji9/Weld-VTK.git
```

All paths in the examples below use relative paths within the repository.

## 1. Overview

The Weld-VTK dataset is intended for research on the relationship between visual features of welding workpieces and human gaze behavior. Typical uses include visual attention modeling, explicit/implicit knowledge analysis, and localization of weld seams or defects. Each image is paired with human gaze recordings and a short description or metadata file.

## 2. Directory structure

At the repository root you will find the following directories:

- `images/`: image files. Filenames typically follow the pattern `<id>_<subid>_<w>_<h>.<ext>` (e.g., `001_1_768_1536.jpg`).
- `gaze/`: CSV files that correspond to images (e.g., `001_1_768_1536.csv`).
- `description/`: text files with annotations or metadata; filenames commonly end with `_description.txt` (e.g., `001_1_768_1536_description.txt`).

Note: naming conventions may vary slightly due to processing; always match files by their base filename.

## 3. File formats and recommended parsing

- `images/`: standard image files; read with common libraries such as Pillow or OpenCV.

- `gaze/*.csv`: typically comma-separated values. Common columns may include (inspect your CSVs to confirm):
  - `timestamp`: recording timestamp (optional).
  - `x`, `y`: gaze coordinates in image or display coordinate system (either pixel values or normalized coordinates — check CSV-specific notes).
  - `validity` / `confidence`: sample validity or confidence (optional).
  - other fields may include `participant_id`, `trial_id`, `eye` (left/right), etc.

- `description/*_description.txt`: plain text files containing short descriptions, cropping/scaling notes, or other annotations. File contents may vary; inspect a few examples before writing a parser.

Because experimental procedures and post-processing can differ, we recommend inspecting a few CSV and description files to determine exact field names and units before bulk processing.

## 4. Pairing images, gaze files and descriptions

Files are typically paired by their base filename. For example:

- Image: `images/001_1_768_1536.jpg`
- Gaze: `gaze/001_1_768_1536.csv`
- Description: `description/001_1_768_1536_description.txt`

Pairing strategy: take the image filename without extension as `base`, look for `gaze/{base}.csv` and `description/{base}_description.txt`.

## 5. Quick usage example (Python)

The following minimal example shows how to read an image and load the corresponding gaze and description files using relative paths. Install `pandas` and `Pillow` first if needed.

```python
import os
import pandas as pd
from PIL import Image

# Use repository-relative paths
root = r"."  # repository root (relative)
base = "001_1_768_1536"  # change to match your file

img_path = os.path.join(root, 'images', base + '.jpg')  # or .png
gaze_path = os.path.join(root, 'gaze', base + '.csv')
desc_path = os.path.join(root, 'description', base + '_description.txt')

# Read image
img = Image.open(img_path)
print('Image size:', img.size)

# Read gaze CSV
gaze_df = pd.read_csv(gaze_path)
print('Gaze head:', gaze_df.head())

# Read description
with open(desc_path, 'r', encoding='utf-8', errors='ignore') as f:
    desc = f.read().strip()
print('Description:', desc)
```

If your images use a different extension than `.jpg`, list `images/` or use `glob` to discover actual filenames.

## 6. Typical analysis workflow

1. Inspect several `gaze` CSVs to determine whether coordinates are in pixels or normalized units and whether coordinate transforms are required.
2. Use information in `description` files (if present) to adjust gaze coordinates for cropping/scaling so they align with image coordinates.
3. Create gaze heatmaps or align gaze data with any available semantic annotations for saliency evaluation.
4. Aggregate gaze traces by participant or trial for statistical analysis or to train machine learning models.

## 7. License and citation

Please acknowledge the data source and original authors when publishing results that use this dataset. Use the following template and replace bracketed fields as appropriate:

```
Weld-VTK dataset, provided by [author/institution]. URL: https://github.com/wangji9/Weld-VTK.git
```

Confirm licensing and attribution requirements with the data provider before redistributing or releasing derivative datasets.

## 8. FAQ

- I cannot find matching files: check the filename base and image extensions (e.g., `_description.txt` suffix). Use a small script to list and compare basenames across directories.
- Gaze CSV columns are inconsistent: read several CSVs first, then write a small normalizer that maps differing column names/units to a common schema.

---

If you want a full dataset summary (counts of images, gaze files, description files, and a list of mismatches), or a runnable Jupyter Notebook that plots a sample gaze heatmap, I can generate those and add them to the repository.


