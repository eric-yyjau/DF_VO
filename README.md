# Introduction

This repo implements the system described in the paper:

[**Visual Odometry Revisited: What Should Be Learnt?** 
](https://arxiv.org/abs/1909.09803) 

Huangying Zhan, Chamara Saroj Weerasekera, Jiawang Bian, Ian Reid

The demo video can be found [here](https://www.youtube.com/watch?v=Nl8mFU4SJKY).

```
@article{zhan2019dfvo,
  title={Visual Odometry Revisited: What Should Be Learnt?},
  author={Zhan, Huangying and Weerasekera, Chamara Saroj and Bian, Jiawang and Reid, Ian},
  journal={arXiv preprint arXiv:1909.09803},
  year={2019}
}
```

<img src='misc/dfvo_eg.gif' width=640 height=320>

This repo includes
1. the frame-to-frame tracking system **DF-VO**;
2. evaluation scripts for visual odometry; 
3. trained models and VO results


### Contents
1. [Requirements](#part-1-requirements)
2. [Prepare dataset](#part-2-download-dataset-and-models)
3. [DF-VO](#part-3-DF-VO)
4. [Result evaluation](#part-4-result-evaluation)


### Part 1. Requirements

This code was tested with Python 3.6, CUDA 9.0, Ubuntu 16.04, and [PyTorch](https://pytorch.org/).

We suggest use [Anaconda](https://www.anaconda.com/distribution/) for installing the prerequisites.

```
conda env create -f requirement.yml -p dfvo # install prerequisites
conda activate dfvo  # activate the environment [dfvo]
```

### Part 2. Download dataset and models

The main dataset used in this project is [KITTI Driving Dataset](http://www.cvlibs.net/datasets/kitti/eval_odometry.php). After downloaing the dataset, create a softlink in the current repo.
```
ln -s KITTI_ODOMETRY/sequences dataset/kitti_odom/odom_data
```

For our trained models, please visit [here](https://www.dropbox.com/sh/9by21564eb0xloh/AABHFMlWd_ja14c5wU4R1KUua?dl=0) to download the models and save the models into the directory `model_zoo/`.

### Part 3. DF-VO
The main algorithm is inplemented in `vo_moduels.py`.
We have created different configurations for running the algrithm.

``` 
# Example 1: run default kitti setup
python run.py -d options/kitti_default_configuration.yml  

# Example 2: Run custom kitti setup
# kitti_default_configuration.yml and kitti_stereo_0.yml are merged
python run.py -c options/kitti_stereo_0.yml  
```

The result (trajectory pose file) is saved in `result_dir` defined in the configuration file.
Please check the `kitti_default_configuration.yml` for more possible configuration.

### Part 4. Result evaluation
<img src='misc/dfvo_result.png' width=320 height=480>

Note that, we have cleaned and optimized the code for better readability and it changes the randomness such that the quantitative result is slightly different from the result reported in the paper. 

<img src='misc/dfvo_result2.png' width=400 height=100>

The original results, including related works, can be found [here](https://www.dropbox.com/sh/u7x3rt4lz6zx8br/AADshjd33Q3TLCy2stKt6qpJa?dl=0).

#### KITTI
[KITTI Odometry benchmark](http://www.cvlibs.net/datasets/kitti/eval_odometry.php) contains 22 stereo sequences, in which 11 sequences are provided with ground truth. The 11 sequences are used for evaluating visual odometry. 

```
python tool/evaluation/eval_odom.py --result result/tmp/0 --align 6dof
```

For more information about the evaluation toolkit, please check the [toolbox page](https://github.com/Huangying-Zhan/kitti_odom_eval) or the [wiki page](https://github.com/Huangying-Zhan/DF-VO/wiki).


### License
For academic usage, the code is released under the permissive MIT license. For any commercial purpose, please contact the authors.


### Acknowledgement
Some of the codes were borrowed from the excellent works of [monodepth2](https://github.com/nianticlabs/monodepth2), [LiteFlowNet](https://github.com/twhui/LiteFlowNet) and [pytorch-liteflownet](https://github.com/sniklaus/pytorch-liteflownet). The borrowed files are licensed under their original license respectively.

### To-do list
- Release more pretrained models
- Release more results
- (maybe) training code: it takes longer time to clean the training code. Also, the current training code involves other projects which increases the difficulty in cleaning the code.