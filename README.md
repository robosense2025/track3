<h1 align="center">🤖 RoboSense Track 3:<br>Sensor Placement</h1>


<div align="center">

**Official Baseline Implementation for [Track 3](https://robosense2025.github.io/track3)**

*Based on Place3D -- "Is Your LiDAR Placement Optimized for 3D Scene Understanding?"*<br>(https://github.com/ywyeli/Place3D)

[![RoboSense Challenge](https://img.shields.io/badge/RoboSense-2025-blue)](https://robosense2025.github.io/)
[![Track](https://img.shields.io/badge/Track-Sensor%20Placement-green)](https://robosense2025.github.io/track3)
[![IROS 2025](https://img.shields.io/badge/IROS-2025-red)](https://iros2025.org/)
[![CodaBench](https://img.shields.io/badge/CodaBench-Submit-purple)](https://www.codabench.org/competitions/9284/)
[![License](https://img.shields.io/badge/License-Apache%202.0-yellow)](LICENSE)

**🏆 Prize Pool: $2,000 USD for Track 3 Winners**

<p align="center">
  <img src="docs/figures/track3.jpg" align="center" width="60%">
</p>

</div>


## Challenge Overview

**Track 3: Sensor Placement** challenges participants to design LiDAR-based 3D perception models, including those for 3D object detection, that can adapt to diverse sensor placements in autonomous systems.

Participants will be tasked with developing novel algorithms that can adapt to and optimize LiDAR sensor placements, ensuring high-quality 3D scene understanding across a wide range of environmental conditions, such as weather variances, motion disturbances, and sensor failures.

### 🎯 Objectives

To be added.


## Competition Details

- **Venue**: IROS 2025, Hangzhou (Oct 19-25, 2025)
- **Registration**: [Google Form](https://docs.google.com/forms/d/e/1FAIpQLSdwfvk-NHdQh9-REiBLCjHMcyLT-sPCOCzJU-ux5jbcZLTkBg/viewform) (Open until Aug 15)
- **Contact**: robosense2025@gmail.com


### 🏆 **Awards**

| Prize | Award |
|:-|:-|
| 🥇 1st Place | $1000 + Certificate |
| 🥈 2nd Place | $600 + Certificate |
| 🥉 3rd Place | $400 + Certificate |
| 🌟 Innovation Award | Cash Award + Certificate |
| Participation | Certificate |



## 📊 Official Dataset

The track 3 [dataset](https://huggingface.co/datasets/robosense/datasets/tree/main/track3-sensor-placement) is prepared using data and settings from the [Place3D](https://github.com/ywyeli/Place3D) framework.


### Dataset Statistics

To be added.



### Baseline Performance (Phase 1)

To be added.



## 🚀 Quick Start

We provide a simple demo to run the baseline model.

### 1. **Preparing conda env**


The code is built with following libraries:

- Python >= 3.8, \<3.9
- OpenMPI = 4.0.4 and mpi4py = 3.0.3 (Needed for torchpack)
- Pillow = 8.4.0 (see [here](https://github.com/mit-han-lab/bevfusion/issues/63))
- [PyTorch](https://github.com/pytorch/pytorch) >= 1.9, \<= 1.10.2
- [tqdm](https://github.com/tqdm/tqdm)
- [torchpack](https://github.com/mit-han-lab/torchpack)
- [mmcv](https://github.com/open-mmlab/mmcv) = 1.4.0
- [mmdetection](http://github.com/open-mmlab/mmdetection) = 2.20.0
- numpy = 1.23

After installing these dependencies, please run this command to install the codebase:

```bash
cd projects/bevfusion
python setup.py develop
```

We also provide a [Dockerfile](projects/bevfusion/docker/Dockerfile) to ease environment setup. To get started with docker, please make sure that `docker` is installed on your machine. After that, please execute the following command to build the docker image:

```bash
cd projects/bevfusion/docker && docker build . -t bevfusion
```

We can then run the docker with the following command:

```bash
docker run --gpus all -it -v `pwd`/../data:/dataset --shm-size 16g bevfusion /bin/bash
```

We recommend the users to run data preparation (instructions are available in the next section) outside the docker if possible. Note that the dataset directory should be an absolute path. Within the docker, please run the following command to clone our repo and install custom CUDA extensions:

```bash
git clone https://github.com/robosense2025/track3 && cd projects/bevfusion
python setup.py develop
```

You can then create a symbolic link `data` to the `/dataset` directory in the docker.

### 2. **Prepare the dataset**

Add `dataset_utils` path to your `PYTHONPATH`. Edit `~/.bashrc` or `~/.zshrc`:

```bash
export PYTHONPATH="$PYTHONPATH:/[YOUR_PARENT_FOLDER]/track3/projects/dataset_utils"
```

Apply the change:
```bash
source ~/.bashrc  # or ~/.zshrc
```

Please download the dataset from [here](https://huggingface.co/datasets/robosense/datasets/tree/main/track3-sensor-placement). Our dataset is in `nuScenes` format, but requires our customized tools.

We typically need to organize the useful data information with a .pkl or .json file in a specific style for organizing annotations.
To prepare these files, run the following command:

```bash
python tools/create_data.py nuscenes --root-path ./data/nuscenes --out-dir ./data/nuscenes --extra-tag nuscenes
```

After data preparation, you will be able to see the following directory structure (as is indicated in mmdetection3d):

```
mmdetection3d
├── mmdet3d
├── tools
├── configs
├── data
│   ├── nuscenes
│   │   ├── maps
│   │   ├── samples
│   │   ├── v1.0-test
|   |   ├── v1.0-trainval
│   │   ├── nuscenes_database
│   │   ├── nuscenes_infos_train.pkl
│   │   ├── nuscenes_infos_val.pkl
│   │   ├── nuscenes_infos_test.pkl
│   │   ├── nuscenes_dbinfos_train.pkl

```

### 3. **Deploy**
To be added.

### Training

For LiDAR detector, please run:

```bash
torchpack dist-run -np 8 python tools/train.py configs/nuscenes/det/transfusion/secfpn/lidar/voxelnet_0p075.yaml
```

### 4. Evaluate the baseline

You will be able to run:

```bash
torchpack dist-run -np 8 python tools/test.py [config file path] pretrained/[checkpoint name].pth --eval [evaluation type]
```

For example:

```bash
torchpack dist-run -np 8 python tools/test.py configs/nuscenes/det/transfusion/secfpn/lidar/voxelnet_0p075.yaml pretrained/track3-baseline.pth --eval bbox
```


## 🎖️ Challenge Participation

### Submission Requirements
To be added.


## 📏 Evaluation Metrics

The evaluation metrics are same as nuScenes. (Do not install the official `nuscenes-devkit`, as it may cause compatibility issues.)



### Timeline
- **Registration**: [Google Form](https://docs.google.com/forms/d/e/1FAIpQLSdwfvk-NHdQh9-REiBLCjHMcyLT-sPCOCzJU-ux5jbcZLTkBg/viewform)
- **Phase 1 Deadline**: August 15th
- **Phase 2 Deadline**: September 15th
- **Awards Announcement**: IROS 2025


## 🔗 Resources

- **Challenge Website**: [robosense2025.github.io](https://robosense2025.github.io/)
- **Track Details**: [Track 3 Page](https://robosense2025.github.io/track3)
- **Track Dataset**: [HuggingFace Page](https://huggingface.co/datasets/robosense/datasets/tree/main/track3-sensor-placement)
- **Baseline Model**: 
- **Related Paper**: [arXiv:2403.17009](https://arxiv.org/abs/2403.17009)



## 📧 Contact & Support

- **Email**: robosense2025@gmail.com
- **Official Website**: https://robosense2025.github.io
- **Issues**: Please use GitHub Issues for technical questions


## 📄 Citation

If you use the code and dataset in your research, please cite:

```bibtex
@inproceedings{li2024place3d,
  title = {Is Your LiDAR Placement Optimized for 3D Scene Understanding?},
  author = {Ye Li and Lingdong Kong and Hanjiang Hu and Xiaohao Xu and Xiaonan Huang},
  booktitle = {Advances in Neural Information Processing Systems},
  year = {2024}
}
```



## Acknowledgements

### RoboSense 2025 Challenge Organizers

<p align="center">
  <img src="docs/figures/organizers.jpg" align="center" width="99%">
</p>


### RoboSense 2025 Program Committee

<p align="center">
  <img src="docs/figures/organizers2.jpg" align="center" width="99%">
</p>



---

<div align="center">

**🤖 Ready to sense the world robustly? Register now and compete for $2,000!**

[**📝 Register Here**](https://docs.google.com/forms/d/e/1FAIpQLSdwfvk-NHdQh9-REiBLCjHMcyLT-sPCOCzJU-ux5jbcZLTkBg/viewform) | [**🌐 Challenge Website**](https://robosense2025.github.io/) | [**📧 Contact Us**](mailto:robosense2025@gmail.com)

Made with ❤️ by the RoboSense 2025 Team

</div>
