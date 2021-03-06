# Metric Learning for Adversarial Robustness


If you find this work is useful, please cite the following:
```
@inproceedings{Mao2019MetricRobust,
title = {Metric Learning for Adversarial Robustness},
author = {Mao, Chengzhi and Zhong, Ziyuan and Yang, Junfeng and Vondrick, Carl and Ray, Baishakhi},
booktitle = {Advances in Neural Information Processing Systems 32},
pages = {478--489},
year = {2019},
}
```

## Requirement
Install tensorflow:    

```
pip install -r requirements.txt
```


All of our experiments are conducted on Amazon AWS EC2 server, with pre-installed tensorflow on the V100 GPU.
If you use AWS server, can activate the conda environment: source activate tensorflow_p36

## MNIST

### Prepare MNIST dataset

run

```
python utils_folder/save_mnist.py
```

### Running experiemnts

All the hyper parameters are set up in `config_mnist.json`

Then run:

```
python train_update_fast_triplet.py
```

To reproduce the TLA algorithm


For baseline models:

Madry et al's   
```
python train_at_madry.py
```

Note that this TLA algorithm takes almost the same time as Madry's baseline to converge, thus patience is needed.

### Evaluations

First, set the path to the directory where the MNIST model is saved. Set up the attack type, the
steps, the step size, and random start.

Then run `python eval.py` to evaluate the model under certain attack


## CIFAR-10


### Prepare CIFAR-10 dataset
Download the data from https://github.com/MadryLab/cifar10_challenge/tree/master/cifar10_data into the
folder cifar10_data

### Running experiemnts

All the hyper parameters are set up in `config_cifar.json`

Then run:

```
python train_update_fast_triplet.py --dataset cifar10
```

To reproduce the ATL algorithm


For baseline models:

Madry et al's   
```
python train_at_madry.py --dataset cifar10
```


### t-SNE
Change the parameters inside the file (especially "mode" and "model_folder_dir") to reproduce results for Figure 1 and
Figure 2, respectively.
```
python tSNE.py
```

### Evaluations

First update the path to the saved CIFAR10 model. Set up the attack type, the
steps, the step size, and random start.

Then run `python eval.py` to evaluate the saved model under the given attack.

## Tiny ImageNet

### Prepare Tiny ImageNet dataset

Download dataset: https://tiny-imagenet.herokuapp.com to subfolder imagenet_data

run
```
python utils_folder/save_imagenet.py
```
to produce preprocessed dataset.

### Running experiemnts


#### Finetuning version
We have Res20 and Res50 architecture option.

set up the config_imagenet.json

first run
```
python train_at_madry.py --dataset imagenet
```


Then set up the finetuning model path in config_imagenet.json, and
run
```
python train_update_fast_triplet.py --dataset imagenet --diff_neg
```


### Evaluations

First update the path to the saved ImageNet model. Set up the attack type, the
steps, the step size, and random start.

Then run `python eval.py` to evaluate the saved model under the given attack.


Notice, AWS need first launch tmux, then activate the tensorflow

Tips: AAP and A1Ap both can give a bit higher performance

For cifar10, when using small models, the negative dictionary size need to decrease such that
the selected negative is not too hard for the metric learning loss.

### Model Weights Download

CIFAR-10: www.cs.columbia.edu/~mcz/publication/upload-cifar-models.zip

Tiny-ImageNet: http://www.cs.columbia.edu/~mcz/publication/TLA-tiny-imagenet.zip
