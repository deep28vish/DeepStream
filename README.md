# Computer Vision using DEEPSTREAM
For complete guide visit- [Computer Vsion In production].

This repository is isolated files from DEEPSTREAM SDK- 5.1
these files when mounted inside NVIDIA-DOCKER- deepstream:5.0.1-20.09-triton. - ```docker pull nvcr.io/nvidia/deepstream:5.1-21.02-triton```
can be used for running inference on 30+ videos in real time.
Minimum Requirement:
* NVIDIA- GRAPHICS DRIVER 440+
* CUDA - 10.2+
* NVIDIA - GPU - GTX, RTX, Pascal, Ampere - 4 Gb minimum
Details about how to use docker / Gstreamer / DeepStream are given in the article.

## Installation from scratch without story
Pre-reqs-Group1:
* Install Latest [NVIDIA-GRAPHICS-DRIVER](https://www.linuxbabe.com/ubuntu/install-nvidia-driver-ubuntu-18-04#:~:text=Then%20open%20softare%20%26%20updates%20program,GeForce%20GTX%201080%20Ti%20card.)

* Install [CUDA-Toolkit](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)

#### To confirm above Installation


```
kuk@kuk:~$ nvidia-smi

+-----------------------------------------------------------------------------+
| NVIDIA-SMI 460.32.03    Driver Version: 460.32.03    CUDA Version: 11.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  GeForce RTX 2060    On   | 00000000:01:00.0  On |                  N/A |
| 32%   35C    P8     1W / 160W |    539MiB /  5931MiB |      2%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+

```
and
```
kuk@kuk:~$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2021 NVIDIA Corporation
Built on Thu_Jan_28_19:32:09_PST_2021
Cuda compilation tools, release 11.2, V11.2.142
Build cuda_11.2.r11.2/compiler.29558016_0
```

Pre-reqs-Group2:

* [Docker Installation](https://docs.docker.com/engine/install/ubuntu/)

```
sudo apt-get update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic test"

sudo apt update

sudo apt install docker-ce

```
#### To confirm Docker installation
```
kuk@kuk:~$ docker -v
Docker version 20.10.6, build 370c289
```

* [Nvidia-Docker-installation](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)

```

distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update

sudo apt-get install -y nvidia-docker2

sudo systemctl restart docker

```
#### To confirm Nvidi-Docker Installation:
```
kuk@kuk:~$ sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
[sudo] password for kuk: 
Unable to find image 'nvidia/cuda:11.0-base' locally
11.0-base: Pulling from nvidia/cuda
54ee1f796a1e: Pull complete 
f7bfea53ad12: Pull complete 
46d371e02073: Pull complete 
b66c17bbf772: Pull complete 
3642f1a6dfb3: Pull complete 
e5ce55b8b4b9: Pull complete 
155bc0332b0a: Pull complete 
Digest: sha256:774ca3d612de15213102c2dbbba55df44dc5cf9870ca2be6c6e9c627fa63d67a
Status: Downloaded newer image for nvidia/cuda:11.0-base
Thu May 13 05:27:44 2021       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 460.32.03    Driver Version: 460.32.03    CUDA Version: 11.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  GeForce RTX 2060    On   | 00000000:01:00.0  On |                  N/A |
| 32%   34C    P8     2W / 160W |    565MiB /  5931MiB |      5%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+
```






## Installation
Use the package manager [pip](https://pip.pypa.io/en/stable/)
[MIT](https://choosealicense.com/licenses/mit/)
