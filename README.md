# Driver phone-use detection

This Neural Computing group project will train a CNN to classify a driver image as phone use or another activity. We use the labelled training images from the [State Farm Distracted Driver Detection competition](https://www.kaggle.com/competitions/state-farm-distracted-driver-detection/data).

## Get the data

1. Sign in to Kaggle and accept the competition rules on the Data page.
2. Download the competition files. The full download is about 4.3 GB. You need `imgs.zip` and `driver_imgs_list.csv`; the competition's unlabelled test images are not needed for our evaluation.
3. Extract the archive and arrange the files inside this repository as follows:

   ```text
   data/
     state-farm/
       driver_imgs_list.csv
       imgs/
         train/
           c0/
           c1/
           ...
           c9/
   ```

The `data/` directory is ignored by Git. Each group member should download the dataset to their own machine or Colab environment; do not commit the image files or archive to GitHub. Keep Kaggle API tokens out of the repository too.

The notebook uses the existing `data/state-farm/` files when present. Otherwise it downloads the competition with KaggleHub. In Colab, add your own `KAGGLE_API_TOKEN` secret and enable notebook access before running; locally, configure Kaggle authentication in your environment. The notebook never contains a token.

In Colab, the results setup cell mounts the runner's Google Drive and saves fold CSV histories and checkpoints under `MyDrive/CS4287_PhoneDetection_results/`. Authorize the mount when prompted. Completed epochs survive a runtime reset, but the current unfinished epoch does not, and the notebook does not automatically resume from a checkpoint. Outside Colab, output goes to `training_results/`, which is ignored by Git. A lecturer running in Colab will mount their own Drive, not yours.

## Local AdamW comparison

Open `CS4287_Assign1_23363819_23362227_23377569_local_AdamW.ipynb` in VS Code and select the `Python (CS4287 AdamW local GPU)` kernel. The prepared environment is in `.venv/` and is ignored by Git; another machine must create its own environment. The local notebook uses the extracted dataset above and writes its outputs to `training_results/adamw_local/`.

The local AdamW notebook uses learning rate 0.001, betas (0.9, 0.99), batch size 32, and 25 epochs across five folds, matching the Adam settings in the group notebook. AdamW uses weight decay 0.0001. The local folds are pinned to the saved Colab fold sizes and driver groups; verify the current Colab Adam run uses the same folds before comparing validation metrics. Run the one-epoch timing cell before starting the full run to estimate its duration on this PC.

The separate `CS4287_Assign1_23363819_23362227_23377569_local_Adam.ipynb` uses the same local dataset and folds with Adam (`weight_decay=0`) and writes to `training_results/adam_local/`. Select the `Python (CS4287 Adam local GPU)` kernel in VS Code. Both local notebooks use the same `.venv`, but each starts its own model and training run.

For our binary task, classes `c1`-`c4` mean texting or talking on a phone; `c0` and `c5`-`c9` mean another activity. Use the driver IDs in `driver_imgs_list.csv` to keep the same driver out of both training and test splits.

## Keep notebook commits clean

Each group member should run these commands once in their own clone of this repository:

```powershell
python -m pip install pre-commit
python -m pre_commit install
```

The configured `nbstripout` hook removes notebook outputs and execution counts when you commit, while keeping code and markdown cells. If the hook changes a notebook, stage it again and repeat the commit. The hook also clears outputs in your local notebook file, so save a separate report figure if you need to keep it. Git does not automatically install hooks when someone clones a repository; each member must run the setup above. This reduces output-only merge conflicts, but it cannot prevent conflicts when two people edit the same cell.
