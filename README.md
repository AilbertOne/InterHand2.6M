
# InterHand2.6M: A Dataset and Baseline for 3D Interacting Hand Pose Estimation from a Single RGB Image

## Introduction
This repo is official **[PyTorch](https://pytorch.org)** implementation of **InterHand2.6M: A Dataset and Baseline for 3D Interacting Hand Pose Estimation from a Single RGB Image (ECCV 2020)**. 

<p align="middle">
<img src="https://drive.google.com/uc?export=view&id=1z9N0FDVyHMmaFpn2-NxXxVBkOtmnND2n" width="260" height="160"><img src="https://drive.google.com/uc?export=view&id=13jImH8aWcY408JLTLSSoFqffRVrJ2nn2" width="260" height="160"><img src="https://drive.google.com/uc?export=view&id=1mW8oPeyUp0woghHOA9EBgo845onrDAU3" width="260" height="160">
</p>
<p align="middle">
<img src="https://drive.google.com/uc?export=view&id=1Z_001absV25Jm8l3kn6EOiaiC5O4QmeC" width="390" height="240"><img src="https://drive.google.com/uc?export=view&id=1Ms1ARdGPNaJa-_mC6dQkIWNtvtxAshUc" width="390" height="240">
</p>

## InterHand2.6M dataset
* For the **InterHand2.6M dataset download and instructions**, go to [DATASET.MD](https://github.com/mks0601/InterHand2.6M/blob/InterNet/DATASET.md). 
* Belows are instructions for **our baseline model**, InterNet, for 3D interacting hand pose estimation from a single RGB image.

## Directory

### Root
The `${ROOT}` is described as below.
```
${ROOT}
|-- data
|-- common
|-- main
|-- output
```
* `data` contains data loading codes and soft links to images and annotations directories.
* `common` contains kernel codes for 3D interacting hand pose estimation.
* `main` contains high-level codes for training or testing the network.
* `output` contains log, trained models, visualized outputs, and test result.

### Data
You need to follow directory structure of the `data` as below.
```
${ROOT}
|-- data
|   |-- STB
|   |   |-- data
|   |   |-- rootnet_output
|   |   |   |-- rootnet_stb_output.json
|   |-- RHD
|   |   |-- data
|   |   |-- rootnet_output
|   |   |   |-- rootnet_rhd_output.json
|   |-- InterHand2.6M
|   |   |-- annotations
|   |   |   |-- all
|   |   |   |-- human_annot
|   |   |   |-- machine_annot
|   |   |-- images
|   |   |   |-- train
|   |   |   |-- val
|   |   |   |-- test
|   |   |-- rootnet_output
|   |   |   |-- rootnet_interhand2.6m_output_all_test.json
|   |   |   |-- rootnet_interhand2.6m_output_machine_annot_val.json
```
* Download InterHand2.6M data [[DATASET.MD](https://github.com/mks0601/InterHand2.6M/blob/InterNet/DATASET.md)]
* Download STB parsed data [[images](https://www.dropbox.com/sh/ve1yoar9fwrusz0/AAAfu7Fo4NqUB7Dn9AiN8pCca?dl=0)] [[annotations](https://drive.google.com/drive/folders/118_FlVoC1Vpil21_zgclz3YiaVTGJ3k-?usp=sharing)]
* Download RHD parsed data [[images](https://lmb.informatik.uni-freiburg.de/resources/datasets/RenderedHandposeDataset.en.html)] [[annotations](https://drive.google.com/drive/folders/1n4WVLTdhNv0FJPYVOXbG5ehu-vpKhcLR?usp=sharing)]
* All annotation files follow [MS COCO format](http://cocodataset.org/#format-data).  
* If you want to add your own dataset, you have to convert it to [MS COCO format](http://cocodataset.org/#format-data).  
  
If you have a problem with 'Download limit' problem when tried to download dataset from google drive link, please try this trick.  
```  
* Go the shared folder, which contains files you want to copy to your drive  
* Select all the files you want to copy  
* In the upper right corner click on three vertical dots and select “make a copy”  
* Then, the file is copied to your personal google drive account. You can download it from your personal account.  
```  

### Output
You need to follow the directory structure of the `output` folder as below.
```
${ROOT}
|-- output
|   |-- log
|   |-- model_dump
|   |-- result
|   |-- vis
```
* `log` folder contains training log file.
* `model_dump` folder contains saved checkpoints for each epoch.
* `result` folder contains final estimation files generated in the testing stage.
* `vis` folder contains visualized results.

## Running InterNet
### Start
* In the `main/config.py`, you can change settings of the model including dataset to use and which root joint translation vector to use (from gt or from [RootNet](https://github.com/mks0601/3DMPPE_ROOTNET_RELEASE)).

### Train
In the `main` folder, run
```bash
python train.py --gpu 0-3 --annot_subset $SUBSET
```
to train the network on the GPU 0,1,2,3. `--gpu 0,1,2,3` can be used instead of `--gpu 0-3`. If you want to continue experiment, run use `--continue`. 

`$SUBSET` is one of [`all`, `human_annot`, `machine_annot`]. 
* `all`: Combination of the human and machine annotation. `Train (H+M)` in the paper.
* `human_annot`: The human annotation. `Train (H)` in the paper.
* `machine_annot`: The machine annotation. `Train (M)` in the paper.


### Test
Place trained model at the `output/model_dump/`.

In the `main` folder, run 
```bash
python test.py --gpu 0-3 --test_epoch 20 --test_set $DB_SPLIT --annot_subset $SUBSET
```
to test the network on the GPU 0,1,2,3 with `snapshot_20.pth.tar`.  `--gpu 0,1,2,3` can be used instead of `--gpu 0-3`. 

`$DB_SPLIT` is one of [`val`,`test`].
* `val`: The validation set. `Val` in the paper.
* `test`: The test set. `Test` in the paper.


`$SUBSET` is one of [`all`, `human_annot`, `machine_annot`].
* `all`: Combination of the human and machine annotation. `(H+M)` in the paper.
* `human_annot`: The human annotation. `(H)` in the paper.
* `machine_annot`: The machine annotation. `(M)` in the paper.

## Results  
Here I provide the performance and pre-trained snapshots of InterNet, and output of the [RootNet](https://github.com/mks0601/3DMPPE_ROOTNET_RELEASE) as well. 
<p align="center">
<img src="https://drive.google.com/uc?export=view&id=1rcA_8f4PrGB_PxBb0vpGW5L5-nOAJMza">
</p>
<p align="center">
<img src="https://drive.google.com/uc?export=view&id=1x1o3JcLXovZhisnlYvqAZasyHn9zUAgi/">
</p>
<p align="center">
<img src="https://drive.google.com/uc?export=view&id=19b2LiqMvfzPTTVxD5VQRFTZyoYDO_Tzd">
</p>

* Pre-trained InterNet trained on InterHand2.6M [[Train \(H\)](https://drive.google.com/drive/folders/1pgbA1K0Bhlhvborgga_naQ3lMsEOX0Cx?usp=sharing)] [[Train \(M\)](https://drive.google.com/drive/folders/1d9dWlGgyEIv0vcMPT1Ox_7lSUGhFXOdN?usp=sharing)] [[Train \(H+M\)](https://drive.google.com/drive/folders/1BET1f5p2-1OBOz6aNLuPBAVs_9NLz5Jo?usp=sharing)]
* Pre-trained InterNet trained on [[STB](https://drive.google.com/drive/folders/178bGDdxfiboZjyMGsi5hE9m0rOFY2RaA?usp=sharing)] [[RHD](https://drive.google.com/drive/folders/159O-_2l2_updTLv4tm4-xVPKrPG74Wsz?usp=sharing)]
* [RootNet](https://github.com/mks0601/3DMPPE_ROOTNET_RELEASE) output on InterHand2.6M [[Val \(M\)](https://drive.google.com/file/d/1DFtB-kvBTr2-WVr_25IlnNU7-PiTpEMu/view?usp=sharing)] [[Test \(H+M\)](https://drive.google.com/file/d/1SSaRcFdws_os_7nBtClKkA8zTZHqGXTj/view?usp=sharing)] [[STB](https://drive.google.com/file/d/1JfHh6J9ORFpBJHUph_BOyEbwP9C4mBY4/view?usp=sharing)] [[RHD](https://drive.google.com/file/d/1rW_T_Qye2Kf0wtBOR5w0FINZ4grZMn6e/view?usp=sharing)]


## Reference  
```  
@InProceedings{Moon_2020_ECCV_InterHand2.6M,  
author = {Moon, Gyeongsik and Yu, Shoou-I and Wen, He and Shiratori, Takaaki and Lee, Kyoung Mu},  
title = {InterHand2.6M: A Dataset and Baseline for 3D Interacting Hand Pose Estimation from a Single RGB Image},  
booktitle = {European Conference on Computer Vision (ECCV)},  
year = {2020}  
}  
```
