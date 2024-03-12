# StyleEmbryo

[12-03-2024]Update: We are happy to announce that Human Reproduction has accepted our paper! :tada:

<p align="center">
  <img src="https://github.com/CellularGenomicMedicine/StyleEmbryo/assets/54368185/e6e39226-f71a-4f4b-b1e0-2ec560d46ccf" width="600">
</p>

This repository was created to provide the original implementation for the paper _"Generative Artificial Intelligence to Produce High-Fidelity Blastocyst-stage Embryo Images"_.
In this repository, you will find the code used for training. Here is a basic summary of the directories in this repository:

- **images/**: Samples of generated images.
- **metrics/**: Scripts for FID/KID calculation.
- **results/**: Visual Turing Test results.
- **training/**: Scripts for training.
- **weights/**: Link to pretrained weights.

# Installation
```
git clone https://github.com/creatorcao/StyleEmbryo.git
cd StyleEmbryo
```

```
conda env create -f environment.yml
conda activate stylegan3
```

# Training 
The training configuration doc in this study can be found here [link](https://github.com/creatorcao/StyleEmbryo/blob/main/training/train_help.txt). 

1. Baseline model
```
python train.py --outdir=./output_gan --data=./embryoGAN256.zip --cfg=stylegan3-t --aug=noaug --gpus=4 --batch=32 --gamma=2 \
    --freezed=13 --workers=2 --mirror=0 --kimg=5000 --tick=2 --snap=50 --metrics=none --cbase=16384 
```

2. Baseline+AUG model
   
```
python train.py --outdir=./output_gan --data=./embryoGAN256.zip --cfg=stylegan3-t --gpus=4 --batch=32 --gamma=2 \
--freezed=13 --workers=2 --mirror=1 --kimg=5000 --tick=2 --snap=50 --metrics=none --cbase=16384 
```

3. Pretrained-T model

```
python train.py --outdir=./output_gan --data=./embryoGAN256.zip --cfg=stylegan3-t --aug=noaug --gpus=4 --batch=32 --gamma=2 \
--freezed=13 --workers=2 --mirror=0 --kimg=5000 --tick=2 --snap=50 --metrics=none --cbase=16384 \
--network=./weights/stylegan3-t-ffhqu-256x256.pkl
```

4. Pretrained-T+AUG model
   
```
python train.py --outdir=./output_gan --data=./embryoGAN256.zip --cfg=stylegan3-t --gpus=4 --batch=32 --gamma=2 \
--freezed=13 --workers=2 --mirror=1 --kimg=5000 --tick=2 --snap=50 --metrics=none --cbase=16384 \
--network=./weights/stylegan3-t-ffhqu-256x256.pkl
```

5. Pretrained-R+AUG model
   
```
python train.py --outdir=./output_gan --data=./embryoGAN256.zip --cfg=stylegan3-r --gpus=4 --batch=32 --gamma=2 \
--freezed=13 --workers=2 --mirror=1 --kimg=5000 --tick=2 --snap=50 --metrics=none --cbase=16384 \
--network=./weights/stylegan3-r-ffhqu-256x256.pkl
```

# Generating image 

```
python gen_images.py --outdir=./images --trunc=1 --seeds=100 \
    --network=./weights/network-snapshot-025000.pkl
```

# Calculating metrics
```
python calc_metrics.py --metrics=fid50k_full,kid50k_full --data=./embryoGAN256.zip --mirror=1 --gpus=1 \
    --network=./weights/network-snapshot-025000.pkl
```
To replicate the result plots, we provided a notebook tutorial. See [Colab notebook](https://github.com/creatorcao/StyleEmbryo/blob/main/figures.ipynb).

# References
- Alias-Free Generative Adversarial Networks. Tero Karras, Miika Aittala, Samuli Laine, Erik Härkönen, Janne Hellsten, Jaakko Lehtinen, Timo Aila. [https://nvlabs.github.io/stylegan3](https://nvlabs.github.io/stylegan3)
