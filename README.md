# Network Dissection

## Introduction
This repository contains the demo code for the [CVPR'17 paper](http://netdissect.csail.mit.edu/final-network-dissection.pdf) Network Dissection: Quantifying Interpretability of Deep Visual Representations. You can use this code with naive [Caffe](https://github.com/BVLC/caffe), with matcaffe and pycaffe compiled. There are dissection results for several networks at the [project page](http://netdissect.csail.mit.edu/).

This code includes

* Code to run network dissection on an arbitrary deep convolutional
    neural network provided as a Caffe deploy.prototxt and .caffemodel.
    The script `rundissect.sh` runs all the needed phases.

* Code to create the merged Broden dataset from constituent datasets
    ADE, PASCAL, PASCAL Parts, PASCAL Context, OpenSurfaces, and DTD.
    The script `makebroden.sh` runs all the needed steps.


## Download
* Clone the code of Network Dissection from github
```
    https://github.com/CSAILVision/NetDissect.git
    cd NetDissect
```
* Download the Broden dataset (~1GB space) and the example pretrained models. 
```
    script/dlbroden_227.sh
    script/dlzoo_example.sh
```

Note that you can run ```script/dlbroden.sh``` to download Broden dataset with images in all three resolution (227x227,224x224,384x384), or run ```script/dlzoo.sh``` to download more CNN models. AlexNet models work with 227x227 image input, while VGG, ResNet, GoogLeNet works with 224x224 image input.

## Run
* Run Network Dissection to probe the conv5 layer of the AlexNet trained on Places365. Results will be saved to ```dissection/caffe_reference_model_places365/```, in which ```html``` contains the visualization of all the units in a html page and ```conv5-result.csv``` contains the raw predicted labels for each unit. The code takes about 40 mintues to run, and it will generate about 1.5GB intermediate results (mmap) for one layer, which you could delete after the code finishes running.

```
    script/rundissect.sh --model caffe_reference_places365 --layers "conv5" --dataset dataset/broden1_227 --resolution 227
```

* Run Network Dissection to compare three layers of AlexNet trained on ImageNet. Results will be saved to ```dissection/caffe_reference_model_imagenet/```. 

```
    script/rundissect.sh --model caffe_reference_imagenet --layers "conv3 conv4 conv5" --dataset dataset/broden1_227 --resolution 227
```

* If you need to regenerate the Broden dataset from scratch, you can run ```script/makebroden.sh```. The script will download the pieces and merge them.

* Network dissection depends on scipy as well as pycaffe. [Details on installing pycaffe can be found here](http://caffe.berkeleyvision.org/tutorial/interfaces.html#python).

## Plot
* After the csv file containing the raw data of the unit semantics is generated, you could use the sample scripts in ```plot/extract_csv.m``` to plot the figure. ```plot/semantics_cvpr_release.mat``` contains the semantics of all the networks analyzed in the CVPR paper. It will generate a [figure](plot/semantics_allnetwork.pdf) showing the number of unique detectors across different networks.


## Reference 
If you find the codes useful, please cite this paper
```
@inproceedings{netdissect2017,
  title={Network Dissection: Quantifying Interpretability of Deep Visual Representations},
  author={Bau, David and Zhou, Bolei and Khosla, Aditya and Oliva, Aude and Torralba, Antonio},
  booktitle={Computer Vision and Pattern Recognition},
  year={2017}
}
```

