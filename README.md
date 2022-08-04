# Julia: VGG16 and ResNet18

## Motivation
The bottleneck of deep learning, which involves large-scale architectures, is training time. Python is a popular language for deep learning. However, it is slow when it handles large volumes of data. Julia is Python-like but is faster than Python because Julia uses Just-In-Time compilation. Simply, Julia combines C and Python. Having said that, there are fewer resources available, and it is difficult to implement a model in Julia. This project shows how to implement VGG16 and ResNet18 from scratch with Cifer10.

## 1. Requirements
- Julia ==> 1.6
- CUDA ==> 3.4.2
- DataFrames ==> 0.21.8
- Flux ==> 0.11.3
- MLDataUtils ==> 0.5.4
- MLDatasets ==> 0.5.11
- Plots ==> 1.21.2

* Julia cares for packages' version. If you see any errors, please make sure the versions are correct.

## 2. Julia Aspects
Q. Is the first run slow?<br>
A. Yes, it will be slow because Julia transfers all of the data to a local GPU.<br>
The below picture shows that the first epoch takes 95 seconds due to the trandering.
<img src="./src/julia1.png" alt="Julia epoch1" title="Julia epoch1">
Then, the second run will be quicker, but the speed depends on your GPU environment.<br>
In our case, Julia redcued the time 9 times from the first epoch.
<img src="./src/epoch2.png" alt="Julia epoch2" title="Julia epoch2">
