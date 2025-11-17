# StudentEngagementRecognition

This repository contains notebooks and scripts used for the engagement-detection experiments. This README explains the recommended execution order and provides copy-paste commands to run the pipeline on macOS (zsh). Run everything from the repository root.

## High-level run order

1. `daisee-seperate-classes.ipynb` — split / reorganize dataset into separate classes (preprocessing)
2. `daisee-augmentation.ipynb` — dataset augmentation (generate augmented images/records)
3. Feature extraction notebooks — extract features from images/videos
   - `daisee-efficient-net-v2-feature-extractor.ipynb` — main feature extractor
   - `daisee-efficient-net-v2-feature-extractor-face.ipynb` — face-specific feature extractor (optional)
4. Training & evaluation notebooks/scripts — train and evaluate models
   - `daisee-efficient-net-lstm.ipynb` — EfficientNet + LSTM training/eval
   - `daisee-efficient-net-transformer.ipynb` — EfficientNet + Transformer training/eval

> There are other notebooks in the repo (for example `data_preprocess.ipynb`) that may be useful for inspecting or preparing data; the list above is the recommended pipeline order.

## Prerequisites

- Python 3.8+ (adjust if your environment requires a different version)
- macOS (instructions use zsh)
- Git
- (Optional) GPU + CUDA for Linux. On macOS you may need CPU or Apple Metal (MPS) support — check the notebook's framework (PyTorch/TensorFlow) and install the appropriate binaries.

## Recommended setup (venv + pip)

From the repository root:

```bash
# create & activate virtual env
python3 -m venv .venv
source .venv/bin/activate

# install dependencies (repository provides requirements.txt)
pip install --upgrade pip
pip install -r requirements.txt

# optional helpers for running notebooks non-interactively
pip install papermill nbconvert
```

If you prefer conda, create a conda env and install packages accordingly.

## Notes and troubleshooting

- Always run from the repository root so notebooks that use relative paths find data and modules.
- Inspect the top cells of each notebook for configurable paths (dataset directory, output paths, hyperparameters). Adjust them before executing non-interactively or pass parameters with papermill if supported.
- Timeouts: long-running notebooks may need higher `--ExecutePreprocessor.timeout` values (the examples show 600–3600 seconds). If a cell hangs, try running the notebook interactively to debug.
- Memory / GPU issues: if training runs out of memory, reduce batch size or use a machine with more RAM/GPU memory. On macOS, native CUDA GPUs are not available — use CPU or Apple MPS (if supported by your ML framework) or run on Linux with CUDA.
- Missing dependencies: if you see import errors, install the missing package into the active virtual environment and re-run.
- Checkpointing: training notebooks may save model checkpoints to a `checkpoints/` or `models/` folder. Inspect the notebooks to find the exact paths.

