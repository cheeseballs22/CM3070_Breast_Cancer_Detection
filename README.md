# CM3070 breast cancer detection

Architecture 5 on CBIS-DDSM: a weight-shared two-view EfficientNet-B3 at 576x448. The patch arm and the direct classifier share that exam network. The patch arm is initialised from a 300x300 three-class Design B pretrain. The direct classifier is initialised from ImageNet. Both are scored on the same 291 JPEG pairs. Rank by test ROC-AUC: patch flip-TTA **0.851**, direct classifier flip-TTA **0.830**.

The written report is `Draft of final report v5.docx`. The brief is `project_specification.txt`.

## Notebooks

Use `.venv` (`requirements.txt` is the install list).

| Notebook | Default run |
|---|---|
| `notebooks/patch_classifier.ipynb` | Scores the patch exam checkpoint |
| `notebooks/direct_classifier.ipynb` | Scores the direct-classifier exam checkpoint |
| `notebooks/comparison_slice_demo.ipynb` | Draws the comparison figures |

The pictures linked from the notebooks are in `notebooks/figures/`, next to the notebooks, so those image links resolve in this repo. The case pictures the demo opens are in `demo/cases/`.

`RUN_SWEEP`, `RUN_WINNING_ARM_TRAIN`, and `RUN_PUSH` stay off. A retrain writes under `weights/direct_classifier_retrain/` or `weights/patch_classifier_push_retrain/`. Set `RUN_DEMO = True` in the demo notebook to rescore the frozen checkpoints and refresh the pictures in `demo/cases/`.

## Weights

The checkpoints are published at https://huggingface.co/ruiner/CM3070_Breast_Cancer_Detection. They are not in this git repo. Download them into `weights/` before a rescore:

```
hf download ruiner/CM3070_Breast_Cancer_Detection --local-dir weights
```

- `weights/patch_classifier/stage2/best_val.pt` - patch exam model (phase 1 epoch 7), flip-TTA 0.851
- `weights/patch_classifier/stage1/best_val.pt` - patch Stage 1
- `weights/patch_classifier/design_b/best_val.pt` - 300x300 three-class head, not the exam model
- `weights/direct_classifier/stage2/best_val.pt` - direct classifier exam model (phase 1 epoch 3), flip-TTA 0.830
- `weights/direct_classifier/stage1/best_val.pt` - direct classifier Stage 1

## Demo

`notebooks/comparison_slice_demo.ipynb` is the visual comparison: the shared network, the ROC and the threshold-0.5 matrices, and four cases with the lesion box beside Direct Classifier Grad-CAM. Open it and scroll. The case PNGs are in `demo/cases/`. The 291 scores are in `demo/scores/`. A normal run does not need a GPU.

## Data

`cbis-ddsm/` is the Kaggle JPEG pack. The frozen splits are `data/train.jsonl`, `val.jsonl`, `test.jsonl`, plus `paired/`, `crops/`, and `enriched/`.

## What is not in this copy

The CBIS-DDSM JPEG pack (`cbis-ddsm/`) stays on the machine where the notebooks were run. The checkpoint files (`best_val.pt`) are on Hugging Face, linked above. The JSON files under `weights/` are the saved metrics. The notebook figures and the demo case pictures are in this repo. Opening `notebooks/comparison_slice_demo.ipynb` draws the figures from `demo/` and `notebooks/figures/` and does not need the checkpoints or the JPEGs.
