# 3D-TC2
This is the official implementation of 3D-TC2(Temporal Consistency Checks to Detect LiDAR Spoofing Attacks on Autonomous Vehicle Perception).

![image](https://user-images.githubusercontent.com/16199843/182342087-c2c67ed6-6ef7-4144-ab9a-3e91aadce45d.png)

Targeting object spoofing attacks, 3D-TC2 can provide more than 98% attack detection rate with a recall of 91% for detecting spoofed Vehicle
(Car) objects, and is able to achieve real-time detection at 41Hz.

Paper Link: https://dl.acm.org/doi/pdf/10.1145/3469261.3469406

Presentation Link: https://youtube.com/watch?v=vkYfP7Cr-1I&feature=share

## Requirements
- CUDA >= 9.0
- Python 3
- PyTorch >= 1.1
- pyquaternion, Matplotlib, PIL, numpy, cv2, tqdm, scipy, scikit-image, scikit-learn, ipython and other relevant packages

## Usage
#### 1. Add path to the root folder. For example:
```
export PYTHONPATH=/your/home/dir/MotionNet:$PYTHONPATH
export PYTHONPATH=/your/home/dir/MotionNet/nuscenes-devkit/python-sdk:$PYTHONPATH
```

#### 2. Dataset

Download Nuscenes dataset from https://www.nuscenes.org/. For example, to download Nuscenes mini split:
```
wget https://www.nuscenes.org/data/v1.0-mini.tgz
```
Download to the dataset folder: `./data/nuscenes/`.

#### 3. Attack the dataset
Attack LiDAR scene in `./data/nuscenes/mini/sweeps/` and customize your own poisoned LiDAR dataset via different data poisoning attack methods.

In our paper, we performed single-frame injection attack and assume historical scenes are not poisoned. To do that, you can randomly pick some scenes from the **temporally attacked dataset** here: Link. Then replace corresponding benign LiDAR scenes in `./data/nuscenes/mini/sweeps/` . 

Note: If you want to replicate our results, please make sure only the scene at current timestamp is poisoned and relatively historical scenes(past 20 scenes) are benign.

#### 4. Object detection
Please feed your poisoned dataset to any kinds of 3D object detectors and get predictions.

Our detection results after running OpenPCDet(https://github.com/open-mmlab/OpenPCDet.git) can be found in `./detection` folder.

#### 5. Motion prediction
Our preliminary implementation of a 3D-TC2 prototype uses pretrained MotionNet to detect anomalies:
```
python TC2.py --data ./data/nuscenes/mini/ --version v1.0-mini --modelpath model.pth --net MotionNet --savepath log
```

If you find `model.pth` corrupted, please download one from MotionNet offical website: https://github.com/pxiangwu/MotionNet .

#### 6. Further exploration
These are some potential directions you might want to further explore:

-Temporal attacks
If you want to perform stress tests on the motion predictor, you can also poison historical scenes to perform consecutive/temporal attacks. To do that, you can select more scenes from  the **temporally attacked dataset** to ensure historical scenes are poisoned to influence motion predictions for the current scene. 

-Other motion predictors. Other pretrained motion predictors such as FlowNet3D(https://github.com/xingyul/flownet3d), PointFlowNet(https://github.com/aseembehl/pointflownet) and HPLFlowNet(https://github.com/laoreja/HPLFlowNet) are also good targets.


## Reference
```
@inproceedings{you2021temporal,
  title={Temporal Consistency Checks to Detect LiDAR Spoofing Attacks on Autonomous Vehicle Perception},
  author={You, Chengzeng and Hau, Zhongyuan and Demetriou, Soteris},
  booktitle={Proceedings of the 1st Workshop on Security and Privacy for Mobile AI},
  pages={13--18},
  year={2021}
}
```
