# 🌽 CornSight

Satellite corn monitoring with NASA/IBM's **Prithvi-EO-2.0** foundation model, USDA data, corn futures and an AI agent. University group project on agentic AI.

## How the project fits together

| Stage | Notebook | Output |
|---|---|---|
| 1. Train the satellite model | `cornsight_train.ipynb` | `training_outputs/` in this repo (published by the notebook's last step) |
| 2. Data pipeline, MongoDB, AI agent | `cornsight_pipeline.ipynb` | Reads `training_outputs/` from this repo; stores all data, charts and AI briefs in MongoDB |

## `training_outputs/`

| File | What it is |
|---|---|
| `chip_results.csv` | One row per validation image (771): location, photo dates, predicted vs. real corn share, corn health (NDVI) per photo |
| `test_scores.csv` | Final accuracy scores (corn IoU 0.533, mIoU 0.427) |
| `learning_curve.png`, `iou_by_class.png`, `corn_acreage.png` | Training and accuracy charts |
| `map_chip_*.png` | Corn maps: satellite photo vs. real crops vs. model prediction |
| `logs/metrics.csv`, `logs/hparams.yaml` | Full training log and model settings |

Model: `prithvi_eo_v2_tiny_tl`, 12 epochs, ~44 min on a free Colab T4 GPU. Dataset: [ibm-nasa-geospatial/multi-temporal-crop-classification](https://huggingface.co/datasets/ibm-nasa-geospatial/multi-temporal-crop-classification). The trained model file (~113 MB) is not included because it exceeds GitHub's 100 MB file limit.
