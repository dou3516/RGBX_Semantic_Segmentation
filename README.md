# RGBX_Semantic_Segmentation

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/semantic-segmentation-on-nyu-depth-v2)](https://paperswithcode.com/sota/semantic-segmentation-on-nyu-depth-v2?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/semantic-segmentation-on-sun-rgbd)](https://paperswithcode.com/sota/semantic-segmentation-on-sun-rgbd?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/semantic-segmentation-on-stanford2d3d)](https://paperswithcode.com/sota/semantic-segmentation-on-stanford2d3d?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/semantic-segmentation-on-scannetv2)](https://paperswithcode.com/sota/semantic-segmentation-on-scannetv2?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/semantic-segmentation-on-cityscapes-val)](https://paperswithcode.com/sota/semantic-segmentation-on-cityscapes-val?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/thermal-image-segmentation-on-mfn-dataset)](https://paperswithcode.com/sota/thermal-image-segmentation-on-mfn-dataset?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/semantic-segmentation-on-zju-rgb-p)](https://paperswithcode.com/sota/semantic-segmentation-on-zju-rgb-p?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/semantic-segmentation-on-eventscape)](https://paperswithcode.com/sota/semantic-segmentation-on-eventscape?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

[![PWC](https://img.shields.io/endpoint.svg?url=https://paperswithcode.com/badge/cmx-cross-modal-fusion-for-rgb-x-semantic/semantic-segmentation-on-rsmss)](https://paperswithcode.com/sota/semantic-segmentation-on-rsmss?p=cmx-cross-modal-fusion-for-rgb-x-semantic)

![Example segmentation](segmentation.jpg?raw=true "Example segmentation")

The official implementation of CMX: Cross-Modal Fusion for RGB-X Semantic Segmentation with Transformers:
More details can be found in our paper [[**PDF**](https://arxiv.org/pdf/2203.04838.pdf)].


## Usage
### Installation
1. Requirements

- Python 3.7+
- PyTorch 1.7.0 or higher
- CUDA 10.2 or higher

We have tested the following versions of OS and softwares:

- OS: Ubuntu 18.04.6 LTS
- CUDA: 10.2
- PyTorch 1.8.2
- Python 3.8.11

2. Install all dependencies.
Install pytorch, cuda and cudnn, then install other dependencies via:
```shell
pip install -r requirements.txt
```

### Datasets

Orgnize the dataset folder in the following structure:
```shell
<datasets>
|-- <DatasetName1>
    |-- <RGBFolder>
        |-- <name1>.<ImageFormat>
        |-- <name2>.<ImageFormat>
        ...
    |-- <ModalXFolder>
        |-- <name1>.<ModalXFormat>
        |-- <name2>.<ModalXFormat>
        ...
    |-- <LabelFolder>
        |-- <name1>.<LabelFormat>
        |-- <name2>.<LabelFormat>
        ...
    |-- train.txt
    |-- test.txt
|-- <DatasetName2>
|-- ...
```

`train.txt` contains the names of items in training set, e.g.:
```shell
<name1>
<name2>
...
```

For RGB-Depth semantic segmentation, the generation of HHA maps from Depth maps can refer to [https://github.com/charlesCXK/Depth2HHA-python](https://github.com/charlesCXK/Depth2HHA-python).

### Train
1. Pretrain weights:

    Download the pretrained segformer here [pretrained segformer](https://drive.google.com/drive/folders/10XgSW8f7ghRs9fJ0dE-EV8G2E_guVsT5?usp=sharing).

2. Config

    Edit config file in configs directory, including dataset and network settings.

3. Run multi GPU distributed training:
    ```shell
    $ CUDA_VISIBLE_DEVICES="GPU IDs" python -m torch.distributed.launch --nproc_per_node="GPU numbers you want to use" train.py -f "config_file"
    ```

- The tensorboard file is saved in `log_<datasetName>_<backboneSize>/tb/` directory.
- Checkpoints are stored in `log_<datasetName>_<backboneSize>/checkpoints/` directory.

### Evaluation
Run the evaluation by:
```shell
CUDA_VISIBLE_DEVICES="GPU IDs" python eval.py -f "config_file" -d="Device ID" -e="epoch number or range"
```
If you want to use multi GPUs please specify multiple Device IDs (0,1,2...).


## Result
We offer the pre-trained weights on different RGBX datasets (Some weights are not avaiable yet, Due to the difference of training platforms, these weights may not be correctly loaded.):

### NYU-V2(40 categories)
| Architecture | Backbone | mIOU(SS) | mIOU(MS & Flip) | Weight |
|:---:|:---:|:---:|:---:| :---:|
| CMX (SegFormer) | MiT-B2 | 54.1% | 54.4% | [NYU-MiT-B2](https://drive.google.com/file/d/1hlyglGnEB0pnWXfHPtBtCGGlKMDh2K--/view?usp=sharing) |
| CMX (SegFormer) | MiT-B4 | 56.0% | 56.3% |  |
| CMX (SegFormer) | MiT-B5 | 56.8% | 56.9% |  |

### MFNet(9 categories)
| Architecture | Backbone | mIOU | Weight |
|:---:|:---:|:---:|:---:|
| CMX (SegFormer) | MiT-B2 | 58.2% | [MFNet-MiT-B2](https://drive.google.com/file/d/1wtWxUgDk1N1QOhiGhUavBNc1_Bv9gzOM/view?usp=sharing) |
| CMX (SegFormer) | MiT-B4 | 59.7% |  |

### ScanNet-V2(20 categories)
| Architecture | Backbone | mIOU | Weight |
|:---:|:---:|:---:|:---:| 
| CMX (SegFormer) | MiT-B2 | 61.3% | [ScanNet-MiT-B2](https://drive.google.com/file/d/1vWsG_wm5p6NSfCFmoWsCAuyWQh1m8dym/view?usp=sharing) |

### RGB-Event(20 categories)
| Architecture | Backbone | mIOU | Weight |
|:---:|:---:|:---:|:---:| 
| CMX (SegFormer) | MiT-B4 | 64.28% | [RGBE-MiT-B4](https://drive.google.com/file/d/1UEnuzu6fwYTH1DROZ0hmzuboLGs5HdmQ/view?usp=sharing) |

## Publication
If you find this repo useful, please consider referencing the following paper:
```
@article{liu2022cmx,
  title={CMX: Cross-Modal Fusion for RGB-X Semantic Segmentation with Transformers},
  author={Liu, Huayao and Zhang, Jiaming and Yang, Kailun and Hu, Xinxin and Stiefelhagen, Rainer},
  journal={arXiv preprint arXiv:2203.04838},
  year={2022}
}
```

## Acknowledgement

Our code is heavily based on [TorchSeg](https://github.com/ycszen/TorchSeg) and [SA-Gate](https://github.com/charlesCXK/RGBD_Semantic_Segmentation_PyTorch), thanks for their excellent work!

