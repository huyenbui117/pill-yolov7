# Pill Gates - VAIPE: Medicine Pill Image Recognition Challenge
## Config data path

### 1. Create data_config.json file in [inference](inference) folder.
File [data_config.json](inference/data_config.json) contains path to train label and image directories of prescription and pill as well as inference (test) image directories of prescription and pill. Example [data_config.json](inference/data_config.example.json) file.

### 2. About [config.json](inference/config.json) file
There are several configs for trainning. The models trained on these configs are then ensembled in test phase. To train each config, put the content in file [config.json](inference/config.json) 

## Pipeline

1. Preprocess data [preprocess.py](inference/preprocess.py) (Run once)
    ```
    python inference/preprocess.py 
    ```
2. Process OCR [infer_ocr.py](inference/infer_ocr.py) (Run once)
    ```
    python inference/infer_ocr.py 
    ```
3. Train and Inference (Test):
    - Train model with each config: Change config and run [train_vaipe.py](inference/train_vaipe.py)
        ```
        python inference/train_vaipe.py
        ```
    - Inference:  run [infer.py](inference/infer.py) 
        - Download model weight in [release](https://github.com/phamgialinhlx/pill-yolov7/releases/tag/v1.0.0) and place in [runs/train](runs/train/) folder
        - Run
        ```
        python inference/infer.py
        ```

## Docker
### Build docker image
- Change [WANDB_TOKEN] to your wandb token
    ```
    docker build . -t ai4vn:latest --build-arg WANDB_TOKEN=[WANDB_TOKEN]
    ```
### Run 
1. Preprocess
    - Change volume dataset (line 3) to your local directory train and test (infer) dataset.
    - Then run
        ``` 
        bash scripts/preprocess.sh 
        ```
2.  Process OCR
    - Change volume dataset (line 3) to your local directory inference test image.
    - Then run
        ```
        bash scripts/infer_ocr.sh
        ```
3. Train and Inference
    - Train
        - Change file [config.json](inference/config.json) content with each config in folder [cfg/docker](cfg/docker)
        - Then run 
            ```
            bash scripts/train.sh
            ```
    - Inference
        - Download private test model weight in [release](https://github.com/phamgialinhlx/pill-yolov7/releases/tag/v1.0.0) and place in [runs/train](runs/train/) folder
        ```bash
        runs
        └──train
            ├── yolov7_45_deg_40_gen 
            │   └── weights
            │       └── best.pt
            ├── yolov7_45_with_newgen
            │   └── weights
            │       └── best.pt
            ├── yolov7_50_deg_40_gen_400epochs4 
            ├── yolov7-tiny_115_deg_40_gen 
            ├──yolov741
            ├──yolov7_50_deg_40_gen_singlecls_400epochs 
            └── yolov7_45_deg_40_gen_singlecls4
        
        ```
        - Change content of [config.json](inference/config.json) as given in [ensemble_config.json](cfg/docker/ensemble_config.json)
        - Then run
        ```
        bash scripts/infer.sh
        ```
## Results
- Model weight stored in [runs/train](runs/train)
- File [runs/ensemble/results.csv](runs/ensemble/results.csv) is the inference (test) results.

