## ESRGAN (Enhanced SRGAN) 
### Test models
1. Clone this github repo.
```
git clone https://github.com/shannu2204/SRGAN.git
cd SRGAN
```
2. Place your own **low-resolution images** in `./LR` folder. (There are two sample images - baboon and comic).
3. Download pretrained models from [Google Drive](https://drive.google.com/drive/u/0/folders/17VYV_SoZZesU6mbxz2dMAIccSSlqLecY) or [Baidu Drive](https://pan.baidu.com/s/1-Lh6ma-wXzfH8NqeBtPaFQ). Place the models in `./models`. We provide two models with high perceptual quality and high PSNR performance (see [model list](https://github.com/xinntao/ESRGAN/tree/master/models)).
4. Run test. We provide ESRGAN model and RRDB_PSNR model and you can config in the `test.py`.
```
python test.py
```
5. The results are in `./results` folder.
### Network interpolation demo
You can interpolate the RRDB_ESRGAN and RRDB_PSNR models with alpha in [0, 1].

1. Run `python net_interp.py 0.8`, where *0.8* is the interpolation parameter and you can change it to any value in [0,1].
2. Run `python test.py models/interp_08.pth`, where *models/interp_08.pth* is the model path.

<p align="center">
  <img height="400" src="figures/43074.gif">
</p>

## Perceptual-driven SR Results

You can download all the resutls from [Google Drive](https://drive.google.com/drive/folders/1iaM-c6EgT1FNoJAOKmDrK7YhEhtlKcLx?usp=sharing). (:heavy_check_mark: included;  :heavy_minus_sign: not included; :o: TODO)

HR images can be downloaed from [BasicSR-Datasets](https://github.com/xinntao/BasicSR#datasets).

| Datasets |LR | [*ESRGAN*](https://arxiv.org/abs/1809.00219) | [SRGAN](https://arxiv.org/abs/1609.04802) | [EnhanceNet](http://openaccess.thecvf.com/content_ICCV_2017/papers/Sajjadi_EnhanceNet_Single_Image_ICCV_2017_paper.pdf) | [CX](https://arxiv.org/abs/1803.04626) |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Set5 |:heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :o: |
| Set14 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :o: |
| BSDS100 | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark:| :o: |
| [PIRM](https://pirm.github.io/) <br><sup>(val, test)</sup> | :heavy_check_mark: | :heavy_check_mark: | :heavy_minus_sign: | :heavy_check_mark:| :heavy_check_mark: |
| [OST300](https://arxiv.org/pdf/1804.02815.pdf) |:heavy_check_mark: | :heavy_check_mark: | :heavy_minus_sign: | :heavy_check_mark:| :o: |
| urban100 | :heavy_check_mark: | :heavy_check_mark: | :heavy_minus_sign: | :heavy_check_mark:| :o: |
| [DIV2K](https://data.vision.ee.ethz.ch/cvl/DIV2K/) <br><sup>(val, test)</sup> | :heavy_check_mark: | :heavy_check_mark: | :heavy_minus_sign: | :heavy_check_mark:| :o: |

## ESRGAN
We improve the [SRGAN](https://arxiv.org/abs/1609.04802) from three aspects:
1. adopt a deeper model using Residual-in-Residual Dense Block (RRDB) without batch normalization layers.
2. employ [Relativistic average GAN](https://ajolicoeur.wordpress.com/relativisticgan/) instead of the vanilla GAN.
3. improve the perceptual loss by using the features before activation.

In contrast to SRGAN, which claimed that **deeper models are increasingly difficult to train**, our deeper ESRGAN model shows its superior performance with easy training.

<p align="center">
  <img height="120" src="figures/architecture.jpg">
</p>
<p align="center">
  <img height="180" src="figures/RRDB.png">
</p>

## Network Interpolation
We propose the **network interpolation strategy** to balance the visual quality and PSNR.

<p align="center">
  <img height="500" src="figures/net_interp.jpg">
</p>

We show the smooth animation with the interpolation parameters changing from 0 to 1.
Interestingly, it is observed that the network interpolation strategy provides a smooth control of the RRDB_PSNR model and the fine-tuned ESRGAN model.

<p align="center">
  <img height="480" src="figures/81.gif">
  &nbsp &nbsp
  <img height="480" src="figures/102061.gif">
</p>

## Qualitative Results
PSNR (evaluated on the Y channel) and the perceptual index used in the PIRM-SR challenge are also provided for reference.

<p align="center">
  <img src="figures/qualitative_cmp_01.jpg">
</p>
<p align="center">
  <img src="figures/qualitative_cmp_02.jpg">
</p>
<p align="center">
  <img src="figures/qualitative_cmp_03.jpg">
</p>
<p align="center">
  <img src="figures/qualitative_cmp_04.jpg">
</p>

## Ablation Study
Overall visual comparisons for showing the effects of each component in
ESRGAN. Each column represents a model with its configurations in the top.
The red sign indicates the main improvement compared with the previous model.
<p align="center">
  <img src="figures/abalation_study.png">
</p>

## BN artifacts
We empirically observe that BN layers tend to bring artifacts. These artifacts,
namely BN artifacts, occasionally appear among iterations and different settings,
violating the needs for a stable performance over training. We find that
the network depth, BN position, training dataset and training loss
have impact on the occurrence of BN artifacts.
<p align="center">
  <img src="figures/BN_artifacts.jpg">
</p>

## Useful techniques to train a very deep network
We find that residual scaling and smaller initialization can help to train a very deep network. More details are in the Supplementary File attached in our [paper](https://arxiv.org/abs/1809.00219).

<p align="center">
  <img height="250" src="figures/train_deeper_neta.png">
  <img height="250" src="figures/train_deeper_netb.png">
</p>

## The influence of training patch size
We observe that training a deeper network benefits from a larger patch size. Moreover, the deeper model achieves more improvement (∼0.12dB) than the shallower one (∼0.04dB) since larger model capacity is capable of taking full advantage of
larger training patch size. (Evaluated on Set5 dataset with RGB channels.)
<p align="center">
  <img height="250" src="figures/patch_a.png">
  <img height="250" src="figures/patch_b.png">
</p>
