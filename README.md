[![License CC BY-NC-SA 4.0](https://img.shields.io/badge/license-CC4.0-blue.svg)](https://github.com/Ha0Tang/LocalGlobalGAN/blob/master/LICENSE.md)
![Python 3.6](https://img.shields.io/badge/python-3.6-green.svg)
![Packagist](https://img.shields.io/badge/Pytorch-0.4.1-red.svg)
![Last Commit](https://img.shields.io/github/last-commit/Ha0Tang/LocalGlobalGAN)
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-blue.svg)]((https://github.com/Ha0Tang/LocalGlobalGAN/graphs/commit-activity))
![Contributing](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)
![Ask Me Anything !](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)
[![GitHub issues](https://img.shields.io/github/issues/Naereen/StrapDown.js.svg)](https://GitHub.com/Ha0Tang/LocalGlobalGAN/issues/)

![LocalGlobalGAN demo](./imgs/dayton_results.jpg)

# Local and Global GAN (LGGAN) for Cross-View Image Translation

## LGGAN Framework
![Framework](./imgs/LocalGlobalGAN_framework.jpg)

## Local Class-Specific Generation
<p align="center">
  <img src='./imgs/LocalGenerator.jpg' align="middle" width=500/>
</p>

### [Paper](https://arxiv.org/abs/1808) | [Project page](http://disi.unitn.it/~hao.tang/project/LocalGlobalGAN.html)

Joint Adversarial Learning Local Class-Specific and Global Image-Level Generation for Cross-View Image Translation.<br>
[Hao Tang](http://disi.unitn.it/~hao.tang/)<sup>1</sup>, [Dan Xu](http://www.robots.ox.ac.uk/~danxu/)<sup>2</sup>, [Jason J. Corso](https://scholar.google.com/citations?user=g9bV-_sAAAAJ&hl=en)<sup>3</sup>, [Nicu Sebe](https://scholar.google.com/citations?user=stFCYOAAAAAJ&hl=en)<sup>1,4</sup> and [Yan Yan](https://userweb.cs.txstate.edu/~y_y34/)<sup>5</sup>. <br> 
<sup>1</sup>University of Trento, Italy, <sup>2</sup>University of Oxford, UK, <sup>3</sup>University of Michigan, USA, <sup>4</sup>Huawei Technologies Ireland, Ireland, <sup>5</sup>Texas State University, USA.<br>
In 2019.<br>
The repository offers the official implementation of our paper in PyTorch.

![LocalGlobalGAN demo](https://github.com/Ha0Tang/GestureGAN/blob/master/imgs/demo_results.gif)
LGGAN for arbitrary cross-view image tranlation task. Given an image and some novel semantic maps, LGGAN is able to generate the same scene but with different viewpoints from both local and global levels.

### [License](./LICENSE.md)

Copyright (C) 2019 University of Trento, Italy.

All rights reserved.
Licensed under the [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode) (**Attribution-NonCommercial-ShareAlike 4.0 International**)

The code is released for academic research use only. For commercial use, please contact [hao.tang@unitn.it](hao.tang@unitn.it).

## Installation

Clone this repo.
```bash
git clone https://github.com/Ha0Tang/LocalGlobalGAN
cd LocalGlobalGAN/
```

This code requires PyTorch 0.4.1 and python 3.6+. Please install dependencies by
```bash
pip install -r requirements.txt (for pip users)
```
or 

```bash
./scripts/conda_deps.sh (for Conda users)
```

To reproduce the results reported in the paper, you would need a NVIDIA GeForce GTX 1080 Ti GPUs with 11 memory.

## Dataset Preparation

For SVA, Dayton or CVUSA, the datasets must be downloaded beforehand. Please download them on the respective webpages. In addition, we put a few sample images in this [code repo](https://github.com/Ha0Tang/LocalGlobalGAN/tree/master/datasets/samples). Please cite their papers if you use the data. 

**Preparing SVA Dataset**. The dataset can be downloaded [here](http://imagelab.ing.unimore.it/imagelab/page.asp?IdPage=19).
Ground Truth semantic maps are not available for this datasets. We adopt [RefineNet](https://github.com/guosheng/refinenet) trained on CityScapes dataset for generating semantic maps and use them as training data in our experiments. Please cite their papers if you use this dataset.
Train/Test splits for SVA dataset can be downloaded from [here](https://github.com/Ha0Tang/LocalGlobalGAN/tree/master/datasets/sva_split).

**Preparing Dayton Dataset**. The dataset can be downloaded [here](https://github.com/lugiavn/gt-crossview). In particular, you will need to download dayton.zip. 
Ground Truth semantic maps are not available for this datasets. We adopt [RefineNet](https://github.com/guosheng/refinenet) trained on CityScapes dataset for generating semantic maps and use them as training data in our experiments. Please cite their papers if you use this dataset.
Train/Test splits for Dayton dataset can be downloaded from [here](https://github.com/Ha0Tang/SelectionGAN/tree/master/datasets/dayton_split).

**Preparing CVUSA Dataset**. The dataset can be downloaded [here](https://drive.google.com/drive/folders/0BzvmHzyo_zCAX3I4VG1mWnhmcGc), which is from the [page](http://cs.uky.edu/~jacobs/datasets/cvusa/). After unzipping the dataset, prepare the training and testing data as discussed in [SelectionGAN](https://arxiv.org/abs/1904.06807). We also adopt [RefineNet](https://github.com/guosheng/refinenet) trained on CityScapes dataset for generating semantic maps and use them as training data in our experiments. Please cite their papers if you use this dataset.

**Preparing Your Own Datasets**. Each training sample in the dataset will contain {Ia,Ig,Sa,Sg}, where Ia=aerial image, Ig=ground image, Sa=semantic map for aerial image and Sg=semantic map for ground image. Of course, you can use LGGAN for your own datasets and tasks.

## Generating Images Using Pretrained Model

Once the dataset is ready. The result images can be generated using pretrained models.

1. You can download a pretrained model (e.g. sva) with the following script:

```
bash ./scripts/download_lggan_model.sh sva
```
The pretrained model is saved at `./checkpoints/[type]_pretrained`. Check [here](https://github.com/Ha0Tang/LocalGlobalGAN/blob/master/scripts/download_lggan_model.sh) for all the available LGGAN models.

2. Generate images using the pretrained model.
```bash
python test.py --dataroot [path_to_dataset] \
	--name [type]_pretrained \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm batch \
	--gpu_ids 0 \
	--batchSize [BS] \
	--loadSize [LS] \
	--fineSize [FS] \
	--no_flip \
	--eval
```

`[path_to_dataset]` is the path to the dataset. Dataset can be one of `sva`, `dayton_a2g`, `dayton_g2a` and `cvusa`. `[type]_pretrained` is the directory name of the checkpoint file downloaded in Step 1, which should be one of `sva_pretrained`, `dayton_a2g_64_pretrained`, `dayton_g2a_64_pretrained`, `dayton_a2g_pretrained`, `dayton_g2a_pretrained` and `cvusa_pretrained`. 
If you are running on CPU mode, change `--gpu_ids 0` to `--gpu_ids -1`. For [`BS`, `LS`, `FS`], please see `Training & Testing` sections.

Note that testing require large amount of disk space. If you don't have enough space, append `--saveDisk` on the command line.
    
3. The outputs images are stored at `./results/[type]_pretrained/` by default. You can view them using the autogenerated HTML file in the directory.

## Training and Testing New Models

New models can be trained and tested with the following commands.

1. Prepare dataset. 

2. Training & Testing

For SVA dataset:
```bash
export CUDA_VISIBLE_DEVICES=0;
python train.py --dataroot ./datasets/sva_local_global 
	--name sva_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 4 \
	--loadSize 286 \
	--fineSize 256 \
	--no_flip \
	--niter 10 \
	--niter_decay 10 \
	--display_id 0

python test.py --dataroot ./datasets/sva_local_global \
	--name sva_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 4 \
	--loadSize 286 \
	--fineSize 256 \
	--no_flip \
	--eval
```

For CVUSA dataset:
```bash
export CUDA_VISIBLE_DEVICES=1;
python train.py --dataroot ./dataset/cvusa_local_global \
	--name cvusa_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 4 \
	--loadSize 286 \
	--fineSize 256 \
	--no_flip \
	--niter 15 \
	--niter_decay 15 \
	--display_id 0

python test.py --dataroot ./dataset/cvusa_local_global \
	--name cvusa_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 4 \
	--loadSize 286 \
	--fineSize 256 \
	--no_flip \
	--eval
```

For Dayton (a2g direction, 256) dataset:
```bash
export CUDA_VISIBLE_DEVICES=6;
python train.py --dataroot ./datasets/dayton_a2g_local_global \
	--name dayton_a2g_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 4 \
	--loadSize 286 \
	--fineSize 256 \
	--no_flip \
	--display_id 0 \
	--niter 20 \
	--niter_decay 15

python test.py --dataroot ./datasets/dayton_a2g_local_global \
	--name dayton_a2g_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 4 \
	--loadSize 286 \
	--fineSize 256 \
	--no_flip \
	--eval
```

For Dayton (g2a direction, 256) dataset:
```bash
export CUDA_VISIBLE_DEVICES=5;
python train.py --dataroot ./datasets/dayton_g2a_local_global \
	--name dayton_g2a_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 4 \
	--loadSize 286 \
	--fineSize 256 \
	--no_flip \
	--display_id 0 \
	--niter 20 \
	--niter_decay 15

python test.py --dataroot ./datasets/dayton_g2a_local_global \
	--name dayton_g2a_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 4 \
	--loadSize 286 \
	--fineSize 256 \
	--no_flip \
	--eval
```

For Dayton (a2g direction, 64) dataset:
```bash
export CUDA_VISIBLE_DEVICES=7;
python train.py --dataroot ./datasets/dayton_a2g_local_global \
	--name dayton_a2g_64_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 16 \
	--loadSize 72 \
	--fineSize 64 \
	--no_flip \
	--display_id 0 \
	--niter 50 \
	--niter_decay 50

python test.py --dataroot ./datasets/dayton_a2g_local_global \
	--name dayton_a2g_64_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 16 \
	--loadSize 72 \
	--fineSize 64 \
	--no_flip \
	--eval
```

For Dayton (g2a direction, 64) dataset:
```bash
export CUDA_VISIBLE_DEVICES=7;
python train.py --dataroot ./datasets/dayton_g2a_local_global \
	--name dayton_g2a_64_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 16 \
	--loadSize 72 \
	--fineSize 64 \
	--no_flip \
	--display_id 0 \
	--niter 50 \
	--niter_decay 50

python test.py --dataroot ./datasets/dayton_g2a_local_global \
	--name dayton_g2a_64_lggan \
	--model lggan \
	--which_model_netG resnet_9blocks \
	--which_direction AtoB \
	--dataset_mode aligned \
	--norm instance \
	--gpu_ids 0 \
	--batchSize 16 \
	--loadSize 72 \
	--fineSize 64 \
	--no_flip \
	--eval
```

### Training & Testing Tips

When training, there are many options you can specify. Please use `python train.py --help`. The specified options are printed to the console. To specify the number of GPUs to utilize, use `export CUDA_VISIBLE_DEVICES=[GPU_ID]`. If you are running on CPU mode, change `--gpu_ids 0` to `--gpu_ids -1`.

To view training results and loss plots on local computers, set `--display_id` to a non-zero value and run `python -m visdom.server` on a new terminal and click the URL [http://localhost:8097](http://localhost:8097/).
On a remote server, replace `localhost` with your server's name, such as [http://server.trento.cs.edu:8097](http://server.trento.cs.edu:8097).

When testing, use `--how_many` to specify the maximum number of images to generate. By default, it loads the latest checkpoint. It can be changed using `--which_epoch`. Note that testing require large amount of disk space since LGGAN will generate lots of intermediate results. If you don't have enough disk space, append `--saveDisk` on the testing command line.

### Can I continue/resume my training? 
To fine-tune a pre-trained model, or resume the previous training, use the `--continue_train --which_epoch <int> --epoch_count<int+1>` flag. The program will then load the model based on epoch `<int>` you set in `--which_epoch <int>`. Set `--epoch_count <int+1>` to specify a different starting epoch count.

## Code Structure

- `train.py`, `test.py`: the entry point for training and testing.
- `models/lggan_model.py`: creates the networks, and compute the losses.
- `models/networks/`: defines the architecture of all models for LGGAN.
- `options/`: creates option lists using `argparse` package.
- `data/`: defines the class for loading images and controllable structures.

## Evaluation Code

We use several metrics to evaluate the quality of the generated images:

- [Inception Score (IS)](https://github.com/Ha0Tang/SelectionGAN/blob/master/scripts/evaluation/compute_topK_KL.py), need install `python 2.7` 
- [Accuracy](https://github.com/Ha0Tang/SelectionGAN/blob/master/scripts/evaluation/compute_accuracies.py), need install `python 2.7` 
- [KL score](https://github.com/Ha0Tang/SelectionGAN/blob/master/scripts/evaluation/KL_model_data.py), need install `python 2.7` 
- [SSIM, PSNR, SD](https://github.com/Ha0Tang/SelectionGAN/blob/master/scripts/evaluation/compute_ssim_psnr_sharpness.lua), need install `Lua` 
- [LPIPS](https://github.com/richzhang/PerceptualSimilarity)
- [FID](https://github.com/mseitzer/pytorch-fid): suggested by [SPADE](https://github.com/NVlabs/SPADE)
- [mIOU on Cityscapes](https://github.com/fyu/drn)

### Citation
If you use this code for your research, please cite our papers:
```
@inproceedings{tang2019joint,
  title={Joint Adversarial Learning Local Class-Specific and Global Image-Level Generation for Cross-View Image Translation},
  author={Tang, Hao and Xu, Dan and Corso, Jason J. and Sebe, Nicu and Yan, Yan},
  booktitle={arXiv},
  year={2019}
}
```
If you use modules from SelectionGAN paper, please also cite their paper:
```
@inproceedings{tang2019multichannel,
  title={Multi-Channel Attention Selection GAN with Cascaded Semantic Guidancefor Cross-View Image Translation},
  author={Tang, Hao and Xu, Dan and Sebe, Nicu and Wang, Yanzhi and Corso, Jason J. and Yan, Yan},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
  year={2019}
}
```

## Acknowledgments
This source code is inspired by [SelectionGAN](https://github.com/Ha0Tang/SelectionGAN).

## Related Projects

- [SelectionGAN (CVPR 2019, PyTorch)](https://github.com/Ha0Tang/SelectionGAN)
- [X-Fork & X-Seq (CVPR 2018, Torch)](https://github.com/kregmi/cross-view-image-synthesis)
- [Pix2pix (CVPR 2017, PyTorch)](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)
- [CrossNet (CVPR 2017, Tensorflow)](https://github.com/viibridges/crossnet)

## Contributions
If you have any questions/comments/bug reports, feel free to open a github issue or pull a request or e-mail to the author Hao Tang ([hao.tang@unitn.it](hao.tang@unitn.it)).
