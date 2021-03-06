# Sparse Fast Gradient Encryption

Enabling Secure in-Memory Neural Network Computing by Sparse Fast Gradient Encryption
>Published in International Conference on Computer-Aided Design 2019<br/>
>Yi Cai, Xiaoming Chen, Lu Tian, Yu Wang, Huazhong Yang<br/>
>Paper link: https://ieeexplore.ieee.org/document/8942041<br/>
>Contact: caiy17@mails.tsinghua.edu.cn, yu-wang@tsinghua.edu.cn<br/>
>Any questions or discussions are welcomed!<br/>


## Abstract
Neural network (NN) computing is energy-consuming on traditional computing systems, owing to the inherent memory wall bottleneck of the von Neumann architecture and the Moore’s Law being approaching the end. Non-volatile memories (NVMs) have been demonstrated as promising alternatives for constructing computing-in-memory (CiM) systems to accelerate NN computing. However, NVM-based NN computing systems are vulnerable to the confidentiality attacks because the weight parameters persist in memory when the system is powered off, enabling an attacker with physical access to extract the well-trained NN models. The goal of this work is to find a solution for thwarting the confidentiality attacks. We define and model the weight encryption problem. Then we propose an effective framework, containing a sparse fast gradient encryption (SFGE) method and a runtime encryption scheduling (RES) scheme, to guarantee the confidentiality security of NN models with a negligible performance overhead. The experiments demonstrate that only encrypting an extremely small proportion of the weights (e.g., 20 weights per layer in ResNet-101), the NN models can be strictly protected.

## Method
<img src="https://github.com/cai-y13/SFGE/blob/master/pic/method.png" width="500" alt="Method framework"/>

We propose an efficient framework, including:<br/>
- Sparse Fast Gradient Encryption (SFGE): to decide where and how to encrypt the deployed NN models<br/>
- Runtime Encryption Scheduling: to decide when to encrypt/decrypt the NN models<br/>

For more details, please refer to the paper.<br/>

## Highlights
- Functionality: The normal function of the CIM accelerators can be guaranteed.
- Fast Restore & Low Overhead: SFGE only encrypts 20~30 weights per neural layer, thus the decryption/encryption will not consume much time. The instant-on benefit of the non-volatility can be preserved.
- Hard to Crack: As only a extremely small portion of the weights are encrypted, the attackers are hard to find the encrypted weights.


## Main Results
### Encryption Effectiveness
  Model | Dataset | #Encryption / Layer | Encryption Intensity | Baseline (top-1/5) | Encrypted (top-1/5)  
  ----- | -----   | ---- | --- | --- | --- 
  ResNet-18 | ImageNet | 20 | 0.2 | 69.75% / 89.07% | 0.704% / 2.452% 
  ResNet-50 | ImageNet | 30 | 0.1 | 76.13% / 92.86% | 0.438% / 1.540%
  ResNet-101 | ImageNet | 20 | 0.1 | 77.38% / 93.54% | 0.144% / 0.758%
  VGG-16 | ImageNet | 30 | 0.2 | 71.59% / 90.38 | 0.818% / 3.478%
  

### Incremental SFGE (i-SFGE)
We also strengthen the keys by incrementally generating them one by one. <br/>
<img src="https://github.com/cai-y13/SFGE/blob/master/pic/iSFGE.png" width="400" alt="Comparing iSFGE"/>

## Installation
Requirements:<br/>
- PyTorch (=1.0.1)
- Torchvision
- tqdm, json

## Run SFGE
We now support the experiments of the models published in torchvision on ImageNet dataset. Later we will support more tasks (e.g. object detection). You can also add your own model to run the experiments.<br/>

There are two steps to run the experiments.<br/>
First, for generating the keys for each layer, run: <br/>
```
python main_generate_key.py
```

Second, for encrypting the models, run:<br/>
```
python main_encrypt.py
```

You can also modify the configuration in `config.py`. The configurable items include:<br/>
- MODEL
  - NAME: the name of the deployed model, e.g. vgg16, resnet18, resnet50, etc.
- SAMPLE_DATASET: the sample dataset for generating the keys
- VAL_DATASET: the validation dataset for evaluating the encryption effectiveness
- ENCRYPT: the encryption configurations
  - NUM: the number of encrypted weights per layer
  - INTENSITY: the intensity of the perturbation added on the weights
  - INCREMENTAL: if incrementally generating the keys
  - MAX_PERCENT: the maximum percentage of encrypted weights in the layer

## Citation
If you find this project useful for your research, please use the following BibTeX entry.<br/>
```
@inproceedings{cai2019enabling,
  title={Enabling Secure in-Memory Neural Network Computing by Sparse Fast Gradient Encryption},
  author={Cai, Yi and Chen, Xiaoming and Tian, Lu and Wang, Yu and Yang, Huazhong},
  booktitle={2019 IEEE/ACM International Conference on Computer-Aided Design (ICCAD)},
  pages={1--8},
  year={2019},
  organization={IEEE}
}
```
