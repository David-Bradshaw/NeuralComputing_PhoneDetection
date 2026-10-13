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

For our binary task, classes `c1`-`c4` mean texting or talking on a phone; `c0` and `c5`-`c9` mean another activity. Use the driver IDs in `driver_imgs_list.csv` to keep the same driver out of both training and test splits.

## Keep notebook commits clean

Each group member should run these commands once in their own clone of this repository:

```powershell
python -m pip install pre-commit
python -m pre_commit install
```

The configured `nbstripout` hook removes notebook outputs and execution counts when you commit, while keeping code and markdown cells. If the hook changes a notebook, stage it again and repeat the commit. The hook also clears outputs in your local notebook file, so save a separate report figure if you need to keep it. Git does not automatically install hooks when someone clones a repository; each member must run the setup above. This reduces output-only merge conflicts, but it cannot prevent conflicts when two people edit the same cell.
