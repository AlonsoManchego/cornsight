# 🌽 CornSight

Satellite corn monitoring with NASA/IBM's **Prithvi-EO-2.0** foundation model, USDA data, corn futures and an AI agent. University group project on agentic AI.

## How the project fits together

| Stage | Notebook | Output |
|---|---|---|
| 1. Train the satellite model | [`cornsight_train.ipynb`](cornsight_train.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AlonsoManchego/cornsight/blob/main/cornsight_train.ipynb) | `training_outputs/` in this repo (published by the notebook's last step) |
| 2. Data pipeline, MongoDB, AI agent | [`cornsight_pipeline.ipynb`](cornsight_pipeline.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AlonsoManchego/cornsight/blob/main/cornsight_pipeline.ipynb) | Reads `training_outputs/` from this repo; stores all data, charts and AI briefs in MongoDB |

Running the pipeline needs a free Colab T4 GPU and two secrets in Colab's 🔑 Secrets panel: `MONGODB_URI` and `NASS_API_KEY`. Keys are never stored in the notebooks.

## `training_outputs/`

| File | What it is |
|---|---|
| `chip_results.csv` | One row per validation image (771): location, photo dates, predicted vs. real corn share, corn health (NDVI) per photo |
| `test_scores.csv` | Final accuracy scores (corn IoU 0.533, mIoU 0.427) |
| `learning_curve.png`, `iou_by_class.png`, `corn_acreage.png` | Training and accuracy charts |
| `map_chip_*.png` | Corn maps: satellite photo vs. real crops vs. model prediction |
| `logs/metrics.csv`, `logs/hparams.yaml` | Full training log and model settings |

Model: `prithvi_eo_v2_tiny_tl`, 12 epochs, ~44 min on a free Colab T4 GPU. Dataset: [ibm-nasa-geospatial/multi-temporal-crop-classification](https://huggingface.co/datasets/ibm-nasa-geospatial/multi-temporal-crop-classification). The trained model file (~113 MB) is not included because it exceeds GitHub's 100 MB file limit.

## `pipeline_outputs/`

Copies of the pipeline's results for the presentation team. The pipeline itself stores these in MongoDB (charts in GridFS, briefs in the `briefs` collection).

| File | What it is |
|---|---|
| `corn_belt_yield_change.png` | Major corn states by satellite health vs. USDA yield change (the 7-of-8 result) |
| `health_vs_yield.png` | Satellite corn health vs. USDA final yield per state (r = 0.51) |
| `corn_futures_wasde.png` | 2022 corn futures with USDA report days marked |
| `brief_trader.md`, `brief_lender.md` | The two final briefs written by the AI agent (both passed the automatic fact-check) |
