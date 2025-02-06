

# Reflexive Guidance (ReGuide)

[![arXiv](https://img.shields.io/badge/arXiv-2410.14975-FF9999.svg)](https://arxiv.org/abs/2410.14975) [![OpeRreview](https://img.shields.io/badge/OpenReview-ReGuide-6699FF.svg)](https://openreview.net/forum?id=R4h5PXzUuU&referrer=%5BAuthor%20Console%5D(%2Fgroup%3Fid%3DICLR.cc%2F2025%2FConference%2FAuthors%23your-submissions))

Official repository for the ICLR 2025 paper "Reflexive Guidance: Improving OoDD in Vision-Language Models via Self-Guided Image-Adaptive Concept Generation"

This repository provides
1) image lists sampeld from the standard OoD setting benchmarks in [OpenOOD v1.5](https://github.com/Jingkang50/OpenOOD) for the experiments in our paper
   - Specifially, we sampled the CIFAR10 and ImageNet200 benchmarks as:

	  |                | CIFAR10 | ImageNet200 |
	  |:--------------:|:-------:|:-----------:|
	  | Sampling ratio |   25%   |   25%, 5%   |

	  ensuring that the proportion of datasets in each benchmark are maintained. 

2) prompt-response pairs obtained from the main experiments
	  |          |  CIFAR10 | ImageNet200 |
	  |:--------:|:--------:|:-----------:|
	  | Baseline |    25%   |   25%, 5%   |
   	  | ReGuide  |    25%   |      5%     |

We hope that the image lists and prompt-response pairs in this repository can be leveraged to support future research and facilitate thorough comparisons.

## Dataset & Response
The overall structure of this repository is as follows, with the results for each sample located under the model directory.

```sh
dataset
    ├─ cifar10
    │    └─ subset_25%.jsonl         
    └─ imagenet200
response
    ├─ baseline
    │    ├─ cifar10-25%
    │    │    ├─ glm
    │    │    │  ...
    │    │    └─ qwen
    │    ├─ imagenet200-25%
    │    └─ imagenet200-5%
    └─ reguide
        └─ imagenet200-5%
            ├─ stage1
            ├─ stage2
            └─ filtering
```

### Preliminary
Our JSONL files for dataset are reorganized based on benchamarks provided by [OpenOOD](https://github.com/Jingkang50/OpenOOD). You can prepare the whole OpenOOD image lists by following the steps below.

First, create the required data directory structure by running the following command:

```sh
mkdir data
```

Then, you can download the dataset using the data download script provided by [OpenOOD](https://github.com/Jingkang50/OpenOOD). After downloading, please ensure that the `images_classic` and `images_largescale` directories are placed inside the `./data` directory. The directory structure should look like this:

```sh
data
    ├─ images_classic
    │    ├─ cifar10
    │    ├─ cifar100
    │    └─ ...
    └─ images_largesacle
```

`image_id` in our dataset JSONL files are the actual path of images in this OpenOOD directory, for example, `./data/images_classic/cifar10/test/airplane/0001.png`.

### Dataset
For **list of images**, each JSONL file we provide is structured as follows:
- Baseline
```json
{
	'dataset': 
		{
			'label': 
				[
					image_id1, 
					image_id2_, 
					...
				]
		}
}
```

### Response
For **prompt-respons pairs**, each JSONL file we provide is structured as follows for **baseline** and **ReGuide** experiments:

- Baseline
```json
{
	'prompt': 
		{
			'image_id': 'response'
		}
}
```

- ReGuide
```json
{
	'image_id': 
		{
			'prompt': 'response'
		}
}
```

The `image_id` field in the JSONL files corresponds to the actual file paths of the image files as mentioned above. If you followed the preliminary steps above, the `image_id` values will match their actual locations, so you can use them directly.


## Overview

### Abstract
With the recent emergence of foundation models trained on internet-scale data and demonstrating remarkable generalization capabilities, such foundation models have become more widely adopted, leading to an expanding range of application domains. Despite this rapid proliferation, the trustworthiness of foundation models remains underexplored. Specifically, the out-of-distribution detection (OoDD) capabilities of large vision-language models (LVLMs), such as GPT-4o, which are trained on massive multi-modal data, have not been sufficiently addressed. The disparity between their demonstrated potential and practical reliability raises concerns regarding the safe and trustworthy deployment of foundation models. To address this gap, we evaluate and analyze the OoDD capabilities of various proprietary and open-source LVLMs. Our investigation contributes to a better understanding of how these foundation models represent confidence scores through their generated natural language responses. Furthermore, we propose a self-guided prompting approach, termed Reflexive Guidance (ReGuide), aimed at enhancing the OoDD capability of LVLMs by leveraging self-generated image-adaptive concept suggestions. Experimental results demonstrate that our ReGuide enhances the performance of current LVLMs in both image classification and OoDD tasks.

### OoD Detection for LVLMs
<img src="./assets/overview.png">
Given the vast amount and broad domain coverage of data used to train LVLMs, we frame the OoDD problem for LVLMs based on the zero-shot OoDD scenario defined for CLIP. Our prompt consists of four components: a task description, an explanation of the rejection class, guidelines, and examples for the response format.

### ReGuide Framework
<img src="./assets/reguide-framework.png">
We introduce a simple and model-agnostic prompting strategy, Reflexive Guidance (ReGuide), to enhance the OoD detectability of LVLMs. The LVLM’s strong generalization ability has been demonstrated through its performance across various downstream tasks. Therefore, we leverage the LVLM itself to obtain guidance for OoDD from its powerful zero-shot visual recognition capabilities. ReGuide is implemented in a two-stage process: Stage 1 Image-adaptive class suggestions and Stage 2 OoDD with suggested classes.


## Citation
```
@inproceedings{kim2025reflexive,
  title={Reflexible Guidance: Improving OoDD in Vision-Language Models via Self-Guided Image-Adaptive Concept Generation},
  author={Jihyo Kim and Seulbi Lee and Sangheum Hwang},
  booktitle={The Thirteenth International Conference on Learning Representations},
  year={2025}
}
```




