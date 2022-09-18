# Welcome to WiSeR cluster
<center>
  
The WiSeR cluster is built by the [WiSeR group](https://wiser.cas.mcmaster.ca/) of [Mcmaster University](https://www.mcmaster.ca/), based on kubernetes. It's composed of three 3080 desktops and six 3060 desktops. It aims to provide computing services e.g., deep learning inference, distributed training for group members.</center>

# Documentation
<details>
  <summary>Setup</summary>
  
  Please follow the instructions to prepare your desktop. Skip this step if the setup is already done.
- Prepare your operating system (OS)
  - Ubuntu20.04 LTS

- Change the hostname
```python
$ sudo nano /etc/hostname
$ Change the hostname to this format: user-gpu-OS, e.g., renjie-3060-u20, keivan-3080-u20, etc.
# Ctrl+s to save, ctrl+x to exit.
```

- Install GPU driver
```python
$ sudo apt install nvidia-driver-510
# Reboot the system and run nvidia-smi to check if the driver is working.
```

- Install cuda and cudnn
```python
# Download cuda and cudnn
$ cd /home/Downloads
$ wget https://developer.download.nvidia.com/compute/cuda/11.1.0/local_installers/cuda_11.1.0_455.23.05_linux.run
$ sudo sh cuda_11.1.0_455.23.05_linux.run 
$ wget
$ tar -zxvf 

- Install cuda
$ sudo nano ~/.bashrc
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
$ export PATH=$PATH:/usr/local/cuda/bin
$ export CUDA_HOME=/usr/local/cuda
$ source ~/. bashrc

- Install cudnn
$ cd cuda
$ sudo cp -a  include/. /usr/local/cuda/include/
$ sudo cp -a lib64/. /usr/local/cuda/lib64/

# Run nvcc --version to check if cuda is working well.
```

- Install Docker
```python
$ sudo apt update
$ sudo apt install docker.io
```

- Enable GPU in Docker
```python
$ sudo apt install curl
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
$ sudo apt-get update
$ sudo apt-get install -y nvidia-docker2
$ sudo nano /etc/docker/daemon.json
$ add "exec-opts": ["native.cgroupdriver=systemd"], in the first line (DO NOT forget comma).
$ sudo systemctl restart docker
```

- Install kubernetes
```python
$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https ca-certificates curl
$ sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
$ echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
$ sudo apt-get update
$ sudo apt-get install -y kubelet=1.22.11-00  kubeadm=1.22.11-00 kubectl=1.22.11-00
$ sudo apt-mark hold kubelet kubeadm kubectl
```

- Configure kubernetes
```python
$ sudo swapoff -a
$ sudo nano /etc/fstab and comment the "/swapfile" line.
$ sudo nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
$ Add Environment=”cgroup-driver=systemd/cgroup-driver=cgroupfs” after the last “Environment Variable”.
```

- Enable GPU in worker nodes
```python
$ sudo nano /etc/docker/daemon.json
$ add "default-runtime": "nvidia", in the first line (DO NOT forget comma).
$ sudo systemctl restart docker
```
  
- Enable GPU in master nodes
```python
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 \
   && chmod 700 get_helm.sh \
   && ./get_helm.sh
$ helm repo add nvdp https://nvidia.github.io/k8s-device-plugin \
   && helm repo update
$ helm install --generate-name nvdp/nvidia-device-plugin --namespace kube-system
```
</details>

<details>
  <summary>Tutorials</summary>
The cluster is built based on kubernetes, where codes are run in containers. Before you get started, please go through the following tutorials and get familiar with the basic concepts of docker and kubernetes.
  
  - [Docker](https://www.youtube.com/watch?v=3c-iBn73dDE)
  - [Kubernetes](https://www.youtube.com/watch?v=X48VuDVv0do&t=1503s)
</details>
