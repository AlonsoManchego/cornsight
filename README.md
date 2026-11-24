# CornSight

Satellite corn monitoring with NASA/IBM's **Prithvi-EO-2.0** foundation model, USDA data, corn futures and an AI agent.

**Team 2:** Akane Eto, Alonso Manchego, Divisha Chellani, Nil Gaziyeva, Tsai-Ling Chen

**Course:** Agentic AI in Business & Finance, Professor Kurnicki, Hult International Business School

## Key results

- **Satellite model:** corn IoU **0.533** (IBM's published, ~20× larger model: 0.549); predicted vs. real corn share per image correlates at **0.89**.
- **Early warning:** in-season (Jul–Aug 2022) satellite flags matched USDA's final 2022 result in **7 of 8 flagged major corn states**, months before the official numbers. Ohio was the miss, and South Dakota's −11.4% drop was not flagged.
- **AI agent:** an open LLM (Qwen3-4B) queries MongoDB with tools and writes briefs for a trading desk and a farm lender. A fact-checker verifies every number and flag, and a human reviews each brief: the AI drafts, a human approves.

## How the project fits together

| Stage | Notebook | Output |
|---|---|---|
| 1. Train the satellite model | [`cornsight_train.ipynb`](cornsight_train.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AlonsoManchego/cornsight/blob/main/cornsight_train.ipynb) | Model checkpoint and results, saved to Google Drive during the run. The files Notebook 2 needs were then copied into `training_outputs/` in this repo |
| 2. Data pipeline, MongoDB, AI agent | [`cornsight_pipeline.ipynb`](cornsight_pipeline.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/AlonsoManchego/cornsight/blob/main/cornsight_pipeline.ipynb) | Reads `training_outputs/` from this repo; stores all data, charts and AI briefs in MongoDB |

Running the pipeline needs a free Colab T4 GPU and two secrets in Colab's Secrets panel: `MONGODB_URI` and `NASS_API_KEY`. Keys are never stored in the notebooks.

Both notebooks are saved with the outputs from our final Colab runs, so you can read the results without running anything.

## `training_outputs/`

| File | What it is |
|---|---|
| `chip_results.csv` | One row per validation image (771): location, photo dates, predicted vs. real corn share, corn health (NDVI) per photo |
| `test_scores.csv` | Final accuracy scores (corn IoU 0.533, mIoU 0.427) |
| `learning_curve.png`, `iou_by_class.png`, `corn_acreage.png` | Training and accuracy charts |
| `map_chip_*.png` | Corn maps: satellite photo vs. real crops vs. model prediction |
| `logs/metrics.csv`, `logs/hparams.yaml` | Full training log and model settings |

Model: `prithvi_eo_v2_tiny_tl`, 12 epochs, 44.5 min on a free Colab T4 GPU. Dataset: [ibm-nasa-geospatial/multi-temporal-crop-classification](https://huggingface.co/datasets/ibm-nasa-geospatial/multi-temporal-crop-classification). The trained model file (~113 MB) is not included because it exceeds GitHub's 100 MB file limit.

## `pipeline_outputs/`

Copies of the pipeline's results. The pipeline itself stores these in MongoDB (charts in GridFS, briefs in the `briefs` collection).

| File | What it is |
|---|---|
| `corn_belt_yield_change.png` | Major corn states by satellite health vs. USDA yield change (7 of 8 flagged states matched) |
| `health_vs_yield.png` | Satellite corn health vs. USDA final yield per state (r = 0.51) |
| `corn_futures_wasde.png` | 2022 corn futures with USDA report days marked |
| `brief_trader.md`, `brief_lender.md` | The two final briefs written by the AI agent (both passed the automatic fact-check; a team member's review of their reasoning is in the pipeline notebook) |

## AI Use Disclosure

We used generative AI tools to support this project: an AI assistant (Claude) to help review our code and results, check numbers for consistency, and draft and edit the presentation script, and Canva AI to help lay out the slides. The project idea, data pipeline, model training, analysis, and final decisions are our own. All AI-assisted content was reviewed, verified against our notebook outputs, and edited by our team. The Qwen3-4B language model and the Prithvi-EO-2.0 model are part of the CornSight system itself, as described in the project.
