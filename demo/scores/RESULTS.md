# Comparison slice

Figures are drawn by `notebooks/comparison_slice_demo.ipynb`. Case PNGs are in `demo/cases/`.

JPEG n=291, flip TTA, threshold 0.5 for the gallery labels. The patch classifier Stage 2 uses PhotoPairedDataset (bilinear). The direct classifier uses PairedMammogramDataset.

Re-score: patch classifier TTA 0.8512 (archived 0.8510), direct classifier TTA 0.8301.

Full-set buckets at 0.5:

| Bucket | n |
|---|---:|
| both_right | 177 |
| patch_only | 29 |
| dc_only | 44 |
| both_wrong | 41 |

Selected 5 per bucket, 20 unique patients, no shortfall. Patch paint is the official CBIS lesion box (Design B pad 0.20), not a detector. The direct classifier column is Grad-CAM of the class logit on `shared_core[-1]`.
