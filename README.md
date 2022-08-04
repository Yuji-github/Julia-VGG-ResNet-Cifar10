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

*Julia cares for packages' version. If you see any errors, please make sure the versions are correct.

## 2. Julia Aspects
Q. Is the first run slow?<br>
A. Yes, it will be slow because Julia transfers all of the data to a local GPU.<br>
The below picture shows that the first epoch takes 95 seconds due to the transfering.
<img src="./src/julia1.png" alt="Julia epoch1" title="Julia epoch1"><br>
Then, the second run will be quicker, but the speed depends on your GPU environment.<br>
In our case, Julia redcued the time 9 times from the first epoch.
<img src="./src/epoch2.png" alt="Julia epoch2" title="Julia epoch2"><br>

Q. Why does Julia provide OOM?<br>
A. This is due to Just-In-Time compilation.<br>
When you train a model in Julia, each time Julia leaves a cache on your GPU. Thus, the GPU is getting heavier and slower.<br>
To avoid the situation, you should turn off the terminal and make sure there is no cache left on your GPU.
<img src="./src/oom.png" alt="julia oom" title="Julia oom"><br>

## 3. Results

In comparison with Python and Julia, those results are obtained from personal computers.<br>
On GPU, the same hardware and software configurations were used for all models.<br>
Results presented refer to both test and training accuracy and loss scores.<br>

### VGG16
| Language | Epoch | Time (seconds) | Train Accuracy | Train Loss | Test Accuracy | Test Loss |
|:-----|:--------:|:------:|:-----:|:--------:|:------:|------:|
|Julia    | 5     | 17.553        |  0.905          | 0.278       |  0.809         |0.568     |
|Julia    | 45    | 15.105          |  0.998         | 0.006      | 0.837        | 0.9333    |
|Julia    | 50    | 15.056         |  0.997          | 0.006      | 0.838        | 0.953      |
|Python   | 5     | 26             | 0.849         |  0.431      | 0.812        | 0.618    |
|Python   | 45    | 27             | 0.988          | 0.035      | 0.825        | 1.065    |
|Python   | 50    | 27             | 0.990         | 1.033      | 0.830          | 1.033     |

### ResNet18
| Language | Epoch | Time (seconds) | Train Accuracy | Train Loss | Test Accuracy | Test Loss |
|:-----|:--------:|:------:|:-----:|:--------:|:------:|------:|
|Julia    | 5     | 33        |  0.758          | 0.682      | 0.714         | 0.827     |
|Julia    | 45    | 31         | 0.971          | 0.086      | 0.766         | 1.354    |
|Julia    | 50    | 30        | 0.991          | 0.027      | 0.787         | 1.216     |
|Python   | 5     | 30             | 0.726          | 0.799      | 0.580         | 1.254   |
|Python   | 45    | 29             | 0.974          | 0.078      | 0.750         | 1.261    |
|Python   | 50    | 29             | 0.976          | 0.074      | 0.756         | 1.214    |

You can see that the Julia model performed slightly better in terms of loss and accuracy scores than the Python equivalent. Additionally, Julia was approximately 10 seconds faster per epoch iteration. This is likely attributed to the usage of CUDA arrays in Julia. Variables can be moved onto the GPU before training. Alternatively, Python moves variables back and forth from the CPU to the GPU during each epoch. As a result, Julia uses variables more efficiently. Overall, there was no significant difference found between the two models.

### Testing Environment
NVIDIA GeForce GTX 1060 GPU, 6GB GDDR5 RAM, 16GB LPDDR3 RAM, Intel Core i7-8650U quad-core @ 1.9 to 4.2 GHz

## 4. Future Study
Julia performs better than Python when we use VGG for cifer10. However, Julia did not differentiate from Python when it used ResNet18. We assume that the transferred data on the GPU, including ResNet18, is slightly larger to optimize Julia's performance. Thus, we need a better GPU to maxmize the performance.
