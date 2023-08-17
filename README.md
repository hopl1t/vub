# VUB - Variational Upper Bound for the Information Bottleneck Objective

MIT license

This library implements the methods described in the paper 'Tighter Bounds on the Information Bottleneck with Application to Deep Learning'.
The information bottleneck (IB) is an information theoretic approach for machine learning optimization. VUB is an upper bound for the IB objective that is computable and optimizable in stochastic DNN settings. This Library allows the reconstruction of the experiments demonstrated in the paper.

<!-- Link to the paper [ paper](https://add_link). -->  This will be filled once the paper is published


## Get started

Prerequisites:
* Python==3.8.10
* Not tested for Python > 3.8.10
* Will not work for Python < 3.8

Begin by installing the dependencies:

```bash
python --version
3.8.10
git clone ## fill up once paper is accepted ##
cd vub
pip install -r requirements.txt
```

Proceed to prepare the data sets using the prepare_run.py script.
# Notes on data:
Two datasets are supported in this repo: ImageNet image classification and IMDB nlp sentiment analysis.
Both datasets need to be preprocessed before used for training and evaluation.
Preprocessing involves saving a local 'logits dataset' that will be used to train the VIB and VUB classifiers.
This repo comes with a 40MB sample of the original ImageNet dataset. The original experiments presented in the paper used the first 100K images. To get the complete dataset and truely reconstruct the experiments one must sign up and download the original 2012 dataset from [link](https://www.image-net.org) to the datasets/imagenet folder and unzip it there.
The IMDB dataset is downloaded automatically upon running the script.

```bash
python src/prepare_run.py --device <cuda \ cpu> --data-class imagenet
python src/prepare_run.py --device <cuda \ cpu> --data-class imdb
```

This command will download and save a pretrained model and process and create a logits dataset. Both will be stored on your local machine.
After the pretrained models and logits datasets are saved, we proceed to the experiments. The following commands replicate the ones done in the paper: Each loss function (VIB and VUB) are trained pre dataset (ImageNet and IMDB). Each training session includes 3 runs per beta value. ImageNet is run for 100 epochs and IMDB for 150. in both casses the regularization terms are clipped not to surpass the CH term for stable learning.

```bash
python src/train_and_eval_cdlvm.py --device cuda --data-class imagenet --betas 0.1 0.01 0.001 --num-runs 3 --loss-type vib --clip-loss true --num-epochs 100
python src/train_and_eval_cdlvm.py --device cuda --data-class imdb --betas 0.1 0.01 0.001 --num-runs 3 --loss-type vib --clip-loss true --num-epochs 150
python src/train_and_eval_cdlvm.py --device cuda --data-class imagenet --betas 0.1 0.01 0.001 --num-runs 3 --loss-type vub --clip-loss true --num-epochs 100
python src/train_and_eval_cdlvm.py --device cuda --data-class imdb --betas 0.1 0.01 0.001 --num-runs 3 --loss-type vub --clip-loss true --num-epochs 150
```

Note - Please use the same device as in prepare_run.py
To get the Vanilla models' evaluation one can use this repo for the image classification tasks, and the textattack repo for the text attacks:

```bash
python src/train_and_eval_cdlvm.py --device cuda --data-class imagenet --loss-type vanilla
textattack attack --recipe deepwordbug --model bert-base-uncased-imdb --dataset-from-huggingface imdb --num-examples 200
```

## Citing VUB

If you use VUB in your work, please cite our paper [paper] <Add link once published>

```bibtex
@article{,
  author  = {},
  title   = {},
  journal = {},
  year    = {},
  volume  = {},
  number  = {},
  pages   = {},
  url     = {}
}
```