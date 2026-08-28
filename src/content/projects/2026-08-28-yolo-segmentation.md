---
title: "YOLO Document Segmentation for OCR"
excerpt: "Training and evaluation pipeline for a YOLO model on photographed health documents."
teaser: "/images/ultralytics.jpeg"
date: 2026-08-28
tags:
  - Computer vision
  - YOLO
  - OCR
  - Document AI
---

## Overview

This project builds the detection stage of a document digitization workflow. Photographs of Indonesian Buku KIA child records are fed to a YOLO model that learns where each information field sits on the page, and the resulting bounding boxes tell a separate OCR service which regions to crop and read. The repository covers everything upstream of text recognition: dataset preparation, training with focal loss, IoU-based evaluation, and inference that writes predictions back out as YOLO label files.

## Workflow

- Takes raw page images with YOLO-format labels and a `classes.txt` defining the segments.
- Trains a YOLO detector with optional focal loss to keep rare or hard fields from being drowned out by easy ones, and logs mean IoU every epoch through a training callback.
- Evaluates by computing IoU manually for each prediction against ground truth with greedy matching, then reports per-class precision and recall, per-image IoU distributions, and annotated images for visual inspection.
- Runs inference to emit YOLO label files with optional confidence scores, which either hand off to the production OCR service or return to Label Studio as pre-annotations for human correction.

![Diagram Overview of the YOLO Document Segmentation Workflow](/images/yolo-workflow.png)

*Figure 1: Segmentation-to-OCR workflow.*

## Why This Matters

Paper records remain the primary data source for a lot of routine health information.

**Locating the field is the hard part.** Running OCR across a full page returns a wall of text with no reliable way to tell which string was the name, which was the date, and which was a heading. A detection model that finds each field first turns an unstructured page into a set of labeled crops, which is what makes the OCR output structured enough to store.

**A box that is nearly right is still wrong.** Detection benchmarks reward mAP, but the metric that matters downstream is how tightly the box fits. A box clipped a few percent short truncates the last character of an identity number, and a box that is too generous pulls in the neighbouring field's text. This is why the evaluation stage recomputes IoU per prediction.

**Class imbalance is inherent to forms.** Some fields appear on every page, others only on a few, and standard detection loss lets the common fields dominate.

**Separating detection from recognition keeps both deployable.** The trained model is a single artifact handed to an OCR service that runs independently, so the recognition system can be swapped or retrained without touching the detector.

Predictions are written in YOLO format, one line per detected field, with an optional sixth column carrying the confidence score that the OCR service can threshold on:

```text
# class_id  x_center  y_center  width  height  [confidence]
5 0.512340 0.187500 0.640625 0.041667 0.913204
2 0.498750 0.402778 0.703125 0.055556 0.874531
```

## Links

- GitHub repository: [yolo_autolabel](https://github.com/oucru-id/yolo_autolabel)
