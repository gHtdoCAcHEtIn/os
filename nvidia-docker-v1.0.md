## Nvidia-docker (version 1.0) installation

Source: [https://github.com/NVIDIA/nvidia-docker/wiki/Installation-(version-1.0)]

### Prerequisites
The list of prerequisites for running nvidia-docker is described below.
For information on how to install Docker for your Linux distribution, please refer to the Docker documentation. [ https://docs.docker.com/engine/installation ]

* GNU/Linux x86_64 with kernel version > 3.10
* Docker >= 1.9 (official docker-engine, docker-ce or docker-ee only)
* NVIDIA GPU with Architecture > Fermi (2.1)
* NVIDIA drivers >= 340.29 with binary nvidia-modprobe
Your driver version might limit your CUDA capabilities (see CUDA requirements)

### Installing version 1.0

#### Ubuntu distributions
Install the repository for your distribution by following the instructions here.
Install the nvidia-docker package
```
sudo apt-get install nvidia-docker
```

#### CentOS distributions
Install the repository for your distribution by following the instructions here.
Install the nvidia-docker package
```sh
sudo yum install nvidia-docker
```
The package installation will automatically setup the nvidia-docker-plugin and depending on your distribution, register it with the init system.

### Basic usage
```bash
nvidia-docker run --rm nvidia/cuda nvidia-smi
```
