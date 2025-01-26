<center>

# DeepPrivacy2 - A Toolbox for Realistic Image Anonymization
</center>

DeepPrivacy2 is a toolbox for realistic anonymization of humans, including a face and a full-body anonymizer.

**Single Face**

| Original | Anonymized |
| ![](media/original(single_face).jpg) | ![](media/anonymized(single_face).jpg) |

**Multiple Faces**

| Original | Anonymized |
|![](media/original(multiple_faces).jpg)|![](media/anonymized(multiple_faces).jpg)|
<p align="center">

<img width="50%" src="https://www.hukkelas.no/images/anonymization_demo.gif" />

</p>


DeepPrivacy first detects, then recursively anonymization all individuals in an image with a  Generative Adversarial Network (GAN) that synthesizes one individual at a time.
![](docs/media/anonymization_illustration.png)


## Modifications over DeepPrivacy2

This repository improves over the original [DeepPrivacy2](https://github.com/hukkelas/DeepPrivacy2) repository with the following new features:
- **Directory Processing:** Anonymize the entire directory of images with a single command
- **Improved Setup Process:** Streamlined setup process
- **Failure Detection:** Instances of failures are detected and logged in a text file


## Quick Start

### Installation
Prerequisites:
First, use the apt package management tools to update your local package index
```
sudo apt update
```

With the update complete, you can install Git:
```
sudo apt install git
```

You can confirm that you have installed Git correctly by running the following command and checking that you receive relevant output.
```
git --version
```

Install python
```
sudo apt update && sudo apt upgrade -y
sudo apt install python3.12
```

Install essential build tools
```
apt-get install build-essential -y
```
Requirements & Installation:
Install PyTorch via pip
```
pip3 install torch torchvision
pip install -e .
```


### Perform Anonymization:

Run the following command. Replace <source_path> with the directory where the photos are stored and the <output_path> with a path to a directory of choice

**Face Anonymization (Complete directory)**
```
python3 anonymize.py configs/anonymizers/deep_privacy1.py -i <source_path> --output_path <output_path>
```

**Full-Body Anonymization (Single Image)**
```
python3 anonymize.py configs/anonymizers/FB_cse.py -i media/regjeringen.jpg --output_path output.png --visualize
```
**Face Anonymization (Single Image)**
```
python3 anonymize.py configs/anonymizers/face.py -i media/regjeringen.jpg --output_path output.png --visualize
```

### Anonymization Failures:
In a very few cases the GAN script may not be able to perform a successful anonymization, albeit the occurrence is low. In my test, this happened in about ~5-10% of the results, usually when the face covered about 70% of the image like very close up headshots. 

The anonymization module has been modified to check if there were no changes made in a file by performing a md5 hash check on both the input file and output file data, and log all such instances to “non_anonymized.txt” in your source directory. See screenshot below.

There can be 2 ways an image ends up in the non_anonymized list - 
- There were faces in the image but no anonymization was performed (We want to investigate these instances and run blurring the faces in these images)
- There were no faces in the image (this is okay, nothing to do further). 


See the [documentation](https://www.hukkelas.no/deep_privacy2/#/anonymization) for more detailed instructions for anonymization.


## License
This repsitory is released under [Apache 2.0 License](License), except for the following:.

- Code under `sg3_torch_utils/`. This code is modified from [github.com/NVlabs/stylegan2-ada-pytorch](https://github.com/NVlabs/stylegan2-ada-pytorch). Separate license is attached in the directory.
- Detection network: See [Detectron2 License](https://github.com/facebookresearch/detectron2/blob/main/LICENSE).
- All checkpoints follow the license of the datasets. See the respective datasets for more information.
- Code under `dp2/detection/models/vit_pose`. This code is modified from [https://github.com/gpastal24/ViTPose-Pytorch](https://github.com/gpastal24/ViTPose-Pytorch), where code is adapted from OpenMMLab. Original license is [Apache 2-0](https://github.com/open-mmlab/mmpose/blob/master/LICENSE).

## Citation
If you find this repository useful, please cite:
```
@inproceedings{hukkelas23DP2,
  author={Hukkelås, Håkon and Lindseth, Frank},
  booktitle={2023 IEEE/CVF Winter Conference on Applications of Computer Vision (WACV)}, 
  title={DeepPrivacy2: Towards Realistic Full-Body Anonymization}, 
  year={2023},
  volume={},
  number={},
  pages={1329-1338},
  doi={10.1109/WACV56688.2023.00138}}
```
