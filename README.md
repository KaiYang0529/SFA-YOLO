<div align="center">
<p>
<a href="" target="_blank">
<img width="100%" src="" alt="SFA-YOLO Banner">
</a>
</p>
</div>

<br>

<div align="center">
<a href=""><img src="https://img.shields.io/badge/Paper-PDF-blue" alt="Paper"></a>
<a href=""><img src="https://img.shields.io/badge/PyTorch-2.0.0-orange" alt="PyTorch"></a>
<a href=""><img src="https://img.shields.io/badge/CUDA-11.8-green" alt="CUDA"></a>
<a href=""><img src="https://img.shields.io/badge/Dataset-IDID-yellow" alt="Dataset"></a>
<a href=""><img src="https://img.shields.io/badge/License-MIT-lightgrey" alt="License"></a>
<br>
<a href=""><img src="https://img.shields.io/badge/Params-3.1M-red" alt="Params"></a>
<a href=""><img src="https://img.shields.io/badge/FLOPs-7.1G-violet" alt="FLOPs"></a>
<a href=""><img src="https://img.shields.io/badge/FPS-185-cyan" alt="FPS"></a>
</div>

<br>

<div align="center">
SFA‑YOLO: A Lightweight Insulator Defect Detection Network Based on Spatial–Frequency Collaborative Perception and Feature Decoupling for Complex Environments
</div>

<div align="center">
  <a href="">English</a> |
  <a href="">中文</a>
</div>

<br>

<img width="100%" src="" alt="SFA‑YOLO Performance Visualization">

## 📖 Introduction

UAV inspection of transmission lines is frequently disrupted by dust storms, haze and low‑light weather, causing severe optical degradation, weak defect edges and high computing resource overhead.
We propose **SFA‑YOLO**, a lightweight detector improved on YOLOv11 for UAV insulator defect detection under complex weather. Three core modules are proposed:

- **MFPM (Multi‑Frequency/Multi‑scale Feature Pyramid Module)**: 2D‑DCT frequency‑domain feature alignment and re‑weighting, suppress semantic dispersion of cross‑scale features.
- **C2WCA (Wavelet‑enhanced Cross‑Attention Mechanism)**: 2D‑DWT frequency‑domain decoupling, extract and compensate high‑frequency defect edge textures for degraded images.
- **SGF‑Block (Synergistic Gated Flow Block)**: Partial‑channel single‑head self‑attention combined with EPGO dynamic sparse denoising and CGLU local nonlinear compensation, maintain lightweight computation cost.

Experiments on weather‑augmented IDID dataset:
- mAP@0.5: **0.931**
- mAP@[0.5:0.95]: **0.780**
- Params: **3.1 M**, FLOPs: **7.1 G**, FPS: **185**

SFA‑YOLO achieves high‑precision detection under complex interference and supports edge deployment for UAV real‑time inspection.

## 📁 Project Structure
SFA‑YOLO
├── cfg
│ └── sfa‑yolo.yaml
├── datasets
│ └── idid.yaml
├── models
│ ├── mfpm.py
│ ├── c2wca.py
│ ├── sgf_block.py
│ └── sfa_yolo.py
├── utils
│ ├── dct_dwt.py
│ └── grad_cam.py
├── train.py
├── val.py
├── predict.py
├── requirements.txt
├── README.md
└── .gitignore


## 📁 Installation
SFA‑YOLO
```bash
# create conda environment
conda create -n sfayolo python=3.10
conda activate sfayolo

# install pytorch 2.0.0 cuda11.8
pip3 install torch==2.0.0 torchvision==0.15.1 --index-url https://download.pytorch.org/whl/cu118

# install other dependencies
pip install -r requirements.txt

## 📊 Dataset
We use IDID (Insulator Defect Image Dataset).
Original: 1598 UAV insulator inspection images
Weather augmentation: dust‑storm blur, synthetic haze, low‑light attenuation, total 6392 images
Split ratio: train / val / test = 8:1:1
Important: All augmented variants generated from one source image must be assigned to the same subset to prevent information leakage.
Dataset folder layout:
plaintext
IDID
├── images
│   ├── train
│   ├── val
│   └── test
└── labels
    ├── train
    ├── val
    └── test
Please modify datasets/idid.yaml and fill your absolute dataset path.
## 🚀 Training

Hyper‑parameters from paper:
lr: 0.01
momentum: 0.937
weight_decay: 0.0005
batch_size: 54
epochs: 500
optimizer: AdamW
bash
python train.py --cfg cfg/sfa-yolo.yaml --data datasets/idid.yaml --epochs 500 --batch 54
## ✅ Validation
bash
python val.py --weights weights/best.pt --data datasets/idid.yaml
## 🔍 Inference
bash
# inference image folder
python predict.py --weights weights/best.pt --source ./test_images

# single image inference
python predict.py --weights weights/best.pt --source test.jpg
## 📈 Visualization
Detection bounding‑box visualization: use predict.py
Grad‑CAM heatmap visualization:
bash
python utils/grad_cam.py --weights weights/best.pt --img test.jpg
## ⚠️ Notes
Do not upload large files to GitHub: datasets, training weights, runs/ logs are ignored in .gitignore.
2D‑DCT / 2D‑DWT frequency transform relies on scipy.
For UAV embedded deployment (Jetson platform), low‑bit quantization and TensorRT acceleration are recommended for industrial usage.
##📝 Citation
bibtex
@article{yang2026sfayolo,
  title={SFA-YOLO: A Lightweight Insulator Defect Detection Network Based on Spatial–Frequency Collaborative Perception and Feature Decoupling for Complex Environments},
  author={Kai Yang, Yu Zhang, Xiaoyang Song, Jiahui Fan},
  journal={},
  year={2026}
} 
## 🧡 Funding
This research is supported by the Shanxi Provincial Basic Research Program (Free Exploration Category, Youth Fund Project, No. 202403021222297), Shanxi Provincial Science and Technology Program Project (No. 202502ZZ0910016).
##📬 Contact
Corresponding author: Kai Yang
Email: yangkai0529@126.com
Code availability: If you need to use the code, you can obtain it from the corresponding author.


