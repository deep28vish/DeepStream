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


## Working with DEEPSTREM - NVIDIA-DOCKER
Downloading the git repo- [Deep28Vish](https://github.com/deep28vish/DeepStream)

Download Location - /Documents/
```
kuk@kuk:~/Documents$ git clone https://github.com/deep28vish/DeepStream.git
Cloning into 'DeepStream'...
remote: Enumerating objects: 25, done.
remote: Counting objects: 100% (25/25), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 25 (delta 9), reused 25 (delta 9), pack-reused 0
Unpacking objects: 100% (25/25), done.
```

#### Downloading and Making DEEPSTREAM container
```
sudo docker run --runtime=nvidia -it -d -e DISPLAY=$DISPLAY --name Thor -v $HOME/Documents/DeepStream/:/home/ --net=host nvcr.io/nvidia/deepstream:5.1-21.02-triton
```
This will output a string looking like:
````
kuk@kuk:~/Documents$ sudo docker run --runtime=nvidia -it -d -e DISPLAY=$DISPLAY --name Thor -v $HOME/Documents/DeepStream/:/home/ --net=host nvcr.io/nvidia/deepstream:5.1-21.02-triton
[sudo] password for kuk: 
d0b4f57b929575a9a43f11955f14010eb84410e325f007fe9ecc69b3e3db5476
````
Container , our sandbox is ready. Going Inside sand box:
```
kuk@kuk:~/Documents$ xhost +
access control disabled, clients can connect from any host
```
TO ENABLE THE VIDEO OUTPUT, REMEMBER TO RUN THIS EVERYTIME YOU ENTER THE CONTAINER.
```
kuk@kuk:~/Documents$ sudo docker exec -it Thor bash
root@kuk:/opt/nvidia/deepstream/deepstream-5.1# cd /
root@kuk:/# cd home/
root@kuk:/home# ls
Primary_Detector  README.md  config_infer_primary.txt  crowd3.mp4  deep_stream_1_feed.txt  deep_stream_30_feed.txt  deep_stream_40_feed.txt  tracker_so  traffic_4k_edited_2.mp4
```
In the above snippet, we got inside our container named Thor, and went to our mounted(git cloned) folder which is present at home.
#### Actually running the INFERENCE ENGINE
```
deepstream-app -c deep_stream_1_feed.txt --tiledtext
```

```
root@kuk:/home# deepstream-app -c deep_stream_1_feed.txt 
** WARN: <parse_osd:946>: Unknown key 'border-color' for group [osd]

 *** DeepStream: Launched RTSP Streaming at rtsp://localhost:8556/ds-test ***

gstnvtracker: Loading low-level lib at /home/tracker_so/libnvds_mot_klt.so
gstnvtracker: Optional NvMOT_ProcessPast not implemented
gstnvtracker: Optional NvMOT_RemoveStreams not implemented
gstnvtracker: Batch processing is OFF
gstnvtracker: Past frame output is OFF
0:00:10.464814015   193 0x7ff95c0022a0 INFO                 nvinfer gstnvinfer.cpp:619:gst_nvinfer_logger:<primary_gie> NvDsInferContext[UID 1]: Info from NvDsInferContextImpl::deserializeEngineAndBackend() <nvdsinfer_context_impl.cpp:1702> [UID = 1]: deserialized trt engine from :/home/Primary_Detector/resnet10.caffemodel_b1_gpu0_int8.engine
INFO: ../nvdsinfer/nvdsinfer_model_builder.cpp:685 [Implicit Engine Info]: layers num: 3
0   INPUT  kFLOAT input_1         3x368x640       
1   OUTPUT kFLOAT conv2d_bbox     16x23x40        
2   OUTPUT kFLOAT conv2d_cov/Sigmoid 4x23x40         

0:00:10.464873182   193 0x7ff95c0022a0 INFO                 nvinfer gstnvinfer.cpp:619:gst_nvinfer_logger:<primary_gie> NvDsInferContext[UID 1]: Info from NvDsInferContextImpl::generateBackendContext() <nvdsinfer_context_impl.cpp:1806> [UID = 1]: Use deserialized engine model: /home/Primary_Detector/resnet10.caffemodel_b1_gpu0_int8.engine
0:00:10.465616239   193 0x7ff95c0022a0 INFO                 nvinfer gstnvinfer_impl.cpp:313:notifyLoadModelStatus:<primary_gie> [UID 1]: Load new model:/home/config_infer_primary.txt sucessfully

Runtime commands:
	h: Print this help
	q: Quit

	p: Pause
	r: Resume

NOTE: To expand a source in the 2D tiled display and view object details, left-click on the source.
      To go back to the tiled display, right-click anywhere on the window.


**PERF:  FPS 0 (Avg)	
**PERF:  0.00 (0.00)	
** INFO: <bus_callback:181>: Pipeline ready

** INFO: <bus_callback:167>: Pipeline running

KLT Tracker: Expect numTransforms config of 1, got 4
KLT Tracker: Ignoring additional transforms but performance may be slower.
KLT Tracker Init
**PERF:  60.52 (60.39)	
**PERF:  59.81 (60.09)	
**PERF:  60.20 (60.06)	
**PERF:  60.31 (60.19)	

```

#### Similarly there is preconfigured text file for running 30 and 40 streams
```
deepstream-app -c deep_stream_30_feed.txt

deepstream-app -c deep_stream_40_feed.txt
```

![preview image](https://github.com/deep28vish/DeepStream/blob/main/screen_image.png?raw=true)



