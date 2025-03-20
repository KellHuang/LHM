# <span><img src="./assets/LHM_logo_parsing.png" height="35" style="vertical-align: top;"> - Official PyTorch Implementation</span>

[![Project Website](https://img.shields.io/badge/🌐-Project_Website-blueviolet)](https://lingtengqiu.github.io/LHM/)
[![arXiv Paper](https://img.shields.io/badge/📜-arXiv:2503-10625)](https://arxiv.org/pdf/2503.10625)
[![HuggingFace](https://img.shields.io/badge/🤗-HuggingFace_Space-blue)](https://huggingface.co/spaces/DyrusQZ/LHM)
[![Apache License](https://img.shields.io/badge/📃-Apache--2.0-929292)](https://www.apache.org/licenses/LICENSE-2.0)

<p align="center">
  <img src="./assets/LHM_teaser.png" heihgt="100%">
</p>

如果您熟悉中文，可以[阅读中文版本的README](./README_CN.md)
## 📢 Latest Updates
**[March 20, 2025]** Release video motion processing pipeline<br>
**[March 19, 2025]** Local Gradio App.py<br>
**[March 19, 2025]** Gradio Optimization:  Faster and More Stable 🔥🔥🔥 <br>
**[March 15, 2025]** Inference Time Optimization:  30% Faster <br>
**[March 13, 2025]** Initial release with:  
✅ Inference codebase  
✅ Pretrained LHM-0.5B model  
✅ Pretrained LHM-1B model  
✅ Real-time rendering pipeline  
✅ Huggingface Online Demo  

### TODO List 
- [x] Core Inference Pipeline (v0.1) 🔥🔥🔥
- [x] HuggingFace Demo Integration 🤗🤗🤗
- [ ] ModelScope Deployment
- [x] Motion Processing Scripts 
- [ ] Training Codes Release

## 🚀 Getting Started

### Environment Setup
Clone the repository.
```bash
git clone git@github.com:aigc3d/LHM.git
cd LHM
```

Install dependencies by script.
```
# cuda 11.8
sh ./install_cu118.sh

# cuda 12.1
sh ./install_cu121.sh
```
The installation has been tested with python3.10, CUDA 11.8 or CUDA 12.1.

Or you can install dependencies step by step, following [INSTALL.md](INSTALL.md).


### Model Weights 

<span style="color:red">Please note that the model will be downloaded automatically if you do not download it yourself.</span>

| Model | Training Data | BH-T Layers | Link | Inference Time|
| :--- | :--- | :--- | :--- | :--- |
| LHM-0.5B | 5K Synthetic Data| 5 | OSS | 2.01 s |
| LHM-0.5B | 300K Videos + 5K Synthetic Data | 5 | [OSS](https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/LHM/LHM-0.5B.tar) | 2.01 s |
| LHM-0.7B | 300K Videos + 5K Synthetic Data | 10 | OSS | 4.13 s  |
| LHM-1.0B | 300K Videos + 5K Synthetic Data | 15 | [OSS](https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/LHM/LHM-1B.tar) | 6.57 s |

```bash
# Download prior model weights
wget https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/LHM/LHM-0.5B.tar
tar -xvf LHM-0.5B.tar 
wget https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/LHM/LHM-1B.tar
tar -xvf LHM-1B.tar 
```

### Download Prior Model Weights 
```bash
# Download prior model weights
wget https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/LHM/LHM_prior_model.tar
tar -xvf LHM_prior_model.tar 
```

### Data Motion Preparation
We provide the test motion examples, we will update the processing scripts ASAP :).

```bash
# Download prior model weights
wget https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/LHM/motion_video.tar
tar -xvf ./motion_video.tar 
```

After downloading weights and data, the folder of the project structure seems like:
```bash
├── configs
│   ├── inference
│   ├── accelerate-train-1gpu.yaml
│   ├── accelerate-train-deepspeed.yaml
│   ├── accelerate-train.yaml
│   └── infer-gradio.yaml
├── engine
│   ├── BiRefNet
│   ├── pose_estimation
│   ├── SegmentAPI
├── example_data
│   └── test_data
├── exps
│   ├── releases
├── LHM
│   ├── datasets
│   ├── losses
│   ├── models
│   ├── outputs
│   ├── runners
│   ├── utils
│   ├── launch.py
├── pretrained_models
│   ├── dense_sample_points
│   ├── gagatracker
│   ├── human_model_files
│   ├── sam2
│   ├── sapiens
│   ├── voxel_grid
│   ├── arcface_resnet18.pth
│   ├── BiRefNet-general-epoch_244.pth
├── scripts
│   ├── exp
│   ├── convert_hf.py
│   └── upload_hub.py
├── tools
│   ├── metrics
├── train_data
│   ├── example_imgs
│   ├── motion_video
├── inference.sh
├── README.md
├── requirements.txt
```

### 💻 Local Gradio Run
```bash
python ./app.py
```

### 🏃 Inference Pipeline
```bash

# MODEL_NAME: {LHM-500M, LHM-1B}
# bash ./inference.sh ./configs/inference/human-lrm-500M.yaml LHM-500M ./train_data/example_imgs/ ./train_data/motion_video/mimo1/smplx_params
# bash ./inference.sh ./configs/inference/human-lrm-1B.yaml LHM-1B ./exps/releases/video_human_benchmark/human-lrm-1B/step_060000/ ./train_data/example_imgs/ ./train_data/motion_video/mimo1/smplx_params

bash inference.sh ${CONFIG} ${MODEL_NAME} ${IMAGE_PATH_OR_FOLDER}  ${MOTION_SEQ}
```

### Custom Video Motion Processing

- Download model weights for motion processing.
  ```bash
  wget -P ./pretrained_models/human_model_files/pose_estimate https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/LHM/yolov8x.pt

  wget -P ./pretrained_models/human_model_files/pose_estimate https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/LHM/vitpose-h-wholebody.pth
  ```

- Install extra dependencies.
  ```bash
  cd ./engine/pose_estimation
  pip install -v -e third-party/ViTPose
  pip install ultralytics
  ```

- Run the script.
   ```bash
   # python ./engine/pose_estimation/video2motion.py --video_path ./train_data/demo.mp4 --output_path ./train_data/custom_motion

   python ./engine/pose_estimation/video2motion.py --video_path ${VIDEO_PATH} --output_path ${OUTPUT_PATH}

   ```

- Use the motion to drive avatar.
  ```bash
  # bash ./inference.sh ./configs/inference/human-lrm-500M.yaml LHM-500M ./train_data/example_imgs/ ./train_data/custom_motion/demo/smplx_params

  bash inference.sh ${CONFIG} ${MODEL_NAME} ${IMAGE_PATH_OR_FOLDER}  ${OUTPUT_PATH}/${VIDEO_NAME}/smplx_params
  ```

## Compute Metric
We provide some simple script to compute the metrics.
```bash
# download pretrain model into ./pretrained_models/
wget https://virutalbuy-public.oss-cn-hangzhou.aliyuncs.com/share/aigc3d/data/for_lingteng/arcface_resnet18.pth
# Face Similarity
python ./tools/metrics/compute_facesimilarity.py -f1 ${gt_folder} -f2 ${results_folder}
# PSNR 
python ./tools/metrics/compute_psnr.py -f1 ${gt_folder} -f2 ${results_folder}
# SSIM LPIPS 
python ./tools/metrics/compute_ssim_lpips.py -f1 ${gt_folder} -f2 ${results_folder} 
```

## Acknowledgement
This work is built on many amazing research works and open-source projects:
- [OpenLRM](https://github.com/3DTopia/OpenLRM)
- [ExAvatar](https://github.com/mks0601/ExAvatar_RELEASE)
- [DreamGaussian](https://github.com/dreamgaussian/dreamgaussian)

Thanks for their excellent works and great contribution to 3D generation and 3D digital human area.

## Citation 
```
@inproceedings{qiu2025LHM,
  title={LHM: Large Animatable Human Reconstruction Model for Single Image to 3D in Seconds},
  author={Lingteng Qiu and Xiaodong Gu and Peihao Li  and Qi Zuo
     and Weichao Shen and Junfei Zhang and Kejie Qiu and Weihao Yuan
     and Guanying Chen and Zilong Dong and Liefeng Bo 
    },
  booktitle={arXiv preprint arXiv:2503.10625},
  year={2025}
}
```
