.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. _gpuinstall:

Using DeepStack with NVIDIA GPU
=================================

DeepStack GPU Version serves requests 5 - 20 times faster than the CPU version if you have an NVIDIA GPU.

**NOTE: THE GPU VERSION IS ONLY SUPPORTED ON LINUX**

Before you install the GPU Version, you need to follow the steps below.

**Step 1: Setup NVIDIA Drivers and CUDA** 

Install the NVIDIA Driver

`GUIDE: Nvidia Driver Install <http://www.linuxandubuntu.com/home/how-to-install-latest-nvidia-drivers-in-linux/>`_

Install the CUDA Toolkit

`GUIDE: Install CUDA Toolkit <https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html/>`_



**Step 2: Install NVIDIA Docker**

The native docker engine does not support GPU access from containers, however **nvidia-docker2** modifies your docker install
to support GPU access.

Run the commands below to modify the docker engine ::


    curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
    sudo apt-key add -

    distribution=$(. /etc/os-release;echo $ID$VERSION_ID)

    curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
    sudo tee /etc/apt/sources.list.d/nvidia-docker.list

    sudo apt-get update

    sudo apt-get install -y nvidia-docker2

    sudo pkill -SIGHUP dockerd

If you run into issues, you can refer to this `GUIDE <https://devblogs.nvidia.com/gpu-containers-runtime//>`_

**Step 3: Install DeepStack GPU Version** ::

    sudo docker pull deepquestai/deepstack:gpu

**RUN DeepStack with GPU Access**

Once the above steps are complete, when you run deepstack, add the args **--rm --runtime=nvidia** ::

    sudo docker run --rm --runtime=nvidia -e VISION-FACE=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack:gpu

In the above example, activated only the FACE Api. You can activate multiple endpoints simultaneously as seen below ::

    sudo docker run --rm --runtime=nvidia -e VISION-FACE=True -e VISION-DETECTION=True \
    -v localstorage:/datastore -p 80:5000 deepquestai/deepstack:gpu

Here we activated two endpoints at the same time, note that the more endpoints you activate, the more the memory usage of DeepStack.
GPUs have limited memory, hence, you should only activate the features you need.
Your system can hang if the memory is overloaded

