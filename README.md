# AI-waste-detection

## Project Overview
### project tree
```
.
├── datasets
│   ├── k_fold_cv
│   │   ├── fold_0
│   │   │   ├── train
│   │   │   │   ├── images
│   │   │   │   └── labels
│   │   │   └── val
│   │   │       ├── images
│   │   │       └── labels
│   │   ├── ...
│   │   └── fold_4
│   ├── object_bank_for_balancing
│   ├── roboflow
│   ├── roboflow_augmented
│   └── taco_official
├── runs
└── src
    ├── data_processing
    ├── inference
    └── training
```

### datasets/
The `datasets/` folder contains all relevant datasets, both raw and processed.

> [!IMPORTANT]
> Before getting started, you must download the official `TACO` dataset by cloning the TACO repository with the following command:
> ```bash
> git clone https://github.com/pedropro/TACO
> ```
> After following the instructions provided in the official repository to download the dataset,
move the `data` folder into `datasets/taco_official`.
> This dataset will later be used to generate the `object_bank_for_balancing`.

### runs/
The `runs/` directory will store the trained models after execution, following the output format of **Ultralytics**.

### src/
The `src/` directory contains all executable `.ipynb` notebooks.

It includes three main subdirectories: `data_processing/` with scripts for **downloading** and **processing** datasets,  
`training/` for **training** models, and `inference/` for **testing** the models.

> Notebooks are expected to be executed with the **notebook** module launched from the **project root folder**.  
> All paths inside notebooks are relative to the project root.  
> If you plan to execute notebooks with a kernel started in a different directory (e.g., from VSCode directly inside the notebook directory),  
> it is recommended to update the paths (typically in the first few cells) before execution.

## Getting started
1. **install necessary packages**: install necessary packages for Python by running the following command:
```bash
pip install -r requirements.txt
```

2. **download the dataset**: get an API key from Roboflow site and write it into a `.env` in the **root directory** of the project and run all cells of `src/data_processing/download_dataset.ipynb`.

> [!NOTE] 
> The `.env` file must have the following format:
> ```yaml
> API_KEY=your_api_jey
> ```

3. **create the folds for cross validation**: run all the cells of `src/data_processing/fold_generation.ipynb`

4. **augment the folds' training sets**: if you wish to augment the training sets of each fold, as described in the original paper, you can do it by running all the cells of `src/data_processing/roboflow_augmnetation.ipynb`

5. **subsample the folds**: at this point each fold should contain a train set of roughly 27000 images with a uniform distribution of all the classes. In order to **avoid overfitting** and **prohibitive training times** we used a pretrained YOLO and RT-DETR and **subsampled** the training sets to 10000 images each. If you wish to subsample the training sets of each fold you can do it by running all the cells of `src/data_processing/roboflow_sampling.ipynb`

6. **train models**: once you have all the folders you can run the same training pipeline, as described in the original paper, by running all the cells of `src/training/training_pipeline.ipynb` otherwise you can train a single model for testing with `src/training/training.ipynb`.

7. **test models**: you can test the models performance (average mAP@50 on all folds when present) on the original test set (`datasets/roboflow/test`) by running all cells of `src/inference/test.ipynb`.
