---
layout: default
title: PEOD Dataset
---

# PEOD: Pixel‑aligned Event Object Detection Dataset

## Overview

Object detection for challenging scenarios increasingly relies on event cameras to overcome the limited dynamic range and motion blur of conventional frame‑based sensors. **PEOD** is the first large‑scale dataset providing synchronized high‑resolution event streams and RGB images for object detection under challenging conditions. It features:

- **High‑resolution data**: 1280 × 720 pixel‑aligned event streams and RGB frames captured using a coaxial dual‑camera system.
- **Diverse environments**: 120+ driving sequences recorded at 30 Hz across urban, suburban and tunnel scenes, with 57 % of data collected under low light, overexposed or high‑speed conditions.
- **Rich annotations**: 340k manually verified bounding boxes for six object classes (car, bus, truck, two‑wheeler, three‑wheeler and person), synchronized at 30 Hz.
- **High dynamic range**: The event camera offers >87 dB HDR, enabling robust perception under extreme illumination.

## Dataset Structure

Each sequence contains a stream of events and corresponding RGB frames with pixel alignment. Annotations are provided in NumPy format. Data splits are organised into training and test sets.

| Split | Boxes | Notes |
|---|---|---|
| Training | 270 k | Diverse illumination & motion conditions |
| Test | 70 k | Held‑out sequences for benchmarking |

## Benchmark Summary

We evaluated representative detectors on three modalities: event‑only, RGB‑only and Event+RGB fusion. Fusion models achieve the best accuracy while event‑only methods excel under extreme illumination.

| Modality | Best model & mAP (COCO mAP<sub>50:95</sub>) | Insights |
|---|---|---|
| Event | **SMamba – 22.9 %** | State‑space model; high accuracy but heavy computation |
| RGB | **YOLOv8 – 19.3 %** | Strong baseline under normal lighting |
| Event+RGB | **EOLO – 24.2 %** | Fusion leverages complementary cues for robust detection |

For condition‑specific results and more details, please refer to the paper.

## Download

The PEOD dataset will be publicly released soon. Data will be provided in RAW and DAT formats with corresponding annotations in NumPy. Check back here for download links.

## Usage Example

Below is a simple Python snippet illustrating how to load event and RGB data with the provided annotations:

```python
import numpy as np
from pathlib import Path
import cv2

# paths to frames, events and annotation file
frame_dir = Path('PEOD/train/sequence_001/rgb')
event_file = Path('PEOD/train/sequence_001/events.dat')
anno_file = Path('PEOD/train/sequence_001/boxes.npy')

# load one RGB frame
img = cv2.imread(str(frame_dir / '000000.png'))
# load bounding boxes (N × 5: frame_idx, class_id, x, y, w, h)
boxes = np.load(anno_file, allow_pickle=True)

# draw first box on the first frame
first_box = boxes[0]
frame_idx, cls_id, x, y, w, h = first_box
cv2.rectangle(img, (int(x), int(y)), (int(x+w), int(y+h)), (0, 255, 0), 2)
cv2.imshow('Example', img)
cv2.waitKey(0)
```

Further examples and code will be provided upon release.

## Citation

If you use PEOD in your research, please cite our paper:

> **PEOD: Pixel‑aligned High‑Resolution Event‑RGB Dataset for Challenging Object Detection**. AAAI 2025.