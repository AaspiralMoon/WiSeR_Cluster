![wisercluster](https://user-images.githubusercontent.com/93042200/190935864-d6e43f9c-7aa8-4a4f-bf42-e4d8d9f01cbb.jpg)

<h1 align="center">:rocket:Welcome to WiSeR cluster:rocket:</h1>
  
The WiSeR cluster is built by the **[WiSeR group](https://wiser.cas.mcmaster.ca/)** of [Mcmaster University](https://www.mcmaster.ca/), based on kubernetes. It is composed of three 3080 desktops and six 3060 desktops. It aims to provide computing services e.g., deep learning training and distributed training for group members.</center>

<h1 align="center">:rainbow:Documentation:rainbow:</h1>
<details>
  <summary>Setup:doughnut:</summary>
  
  Please follow the instructions to prepare your desktop. Skip this step if the setup is already done.
### Prepare Your Operating System (OS)
  - Ubuntu20.04 LTS

### Change the Hostname
```python
$ sudo nano /etc/hostname
$ Change the hostname to this format: user-gpu-OS, e.g., renjie-3060-u20, keivan-3080-u20, etc.
# Ctrl+s to save, ctrl+x to exit.
```

### Install GPU Driver
```python
$ sudo apt install nvidia-driver-510
# Reboot the system and run nvidia-smi to check if the driver is working.
```

### Install cuda and cudnn
```python
# Download cuda and cudnn
$ cd /home/Downloads
$ wget https://developer.download.nvidia.com/compute/cuda/11.1.0/local_installers/cuda_11.1.0_455.23.05_linux.run
$ sudo sh cuda_11.1.0_455.23.05_linux.run 
$ wget
$ tar -zxvf 

# Install cuda
$ sudo nano ~/.bashrc
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
$ export PATH=$PATH:/usr/local/cuda/bin
$ export CUDA_HOME=/usr/local/cuda
$ source ~/. bashrc

# Install cudnn
$ cd cuda
$ sudo cp -a  include/. /usr/local/cuda/include/
$ sudo cp -a lib64/. /usr/local/cuda/lib64/

# Run nvcc --version to check if cuda is working well.
```

### Install Docker
```python
$ sudo apt update
$ sudo apt install docker.io
```

### Enable GPU in Docker
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

### Install Kubernetes
```python
$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https ca-certificates curl
$ sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
$ echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
$ sudo apt-get update
$ sudo apt-get install -y kubelet=1.22.11-00  kubeadm=1.22.11-00 kubectl=1.22.11-00
$ sudo apt-mark hold kubelet kubeadm kubectl
```

### Configure Kubernetes
```python
$ sudo swapoff -a
$ sudo nano /etc/fstab and comment the "/swapfile" line.
$ sudo nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
$ Add Environment=”cgroup-driver=systemd/cgroup-driver=cgroupfs” after the last “Environment Variable”.
```

### Enable GPU in Kubernetes

- Run on worker nodes

```python
$ sudo nano /etc/docker/daemon.json
$ add "default-runtime": "nvidia", in the first line (DO NOT forget comma).
$ sudo systemctl restart docker
```
  
- Run on master nodes

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
  <summary>Tutorials:cake:</summary>
  
The cluster is built based on kubernetes, where codes are run in containers. Before you get started, please go through the following tutorials and get familiar with the basic concepts of [docker](https://www.docker.com/) and [kubernetes](https://kubernetes.io/). Do not be afraid, they are easy! :smile:
  
  - [Docker](https://www.youtube.com/watch?v=3c-iBn73dDE)
  - [Kubernetes](https://www.youtube.com/watch?v=X48VuDVv0do&t=1503s)
</details>

<details>
  <summary>Services:icecream:</summary>
  
All services supported by the cluster are listed as follows. Currently training and distributed training have been tested. Non-DL workloads, e.g., matlab, c++, should be supported as well. If you have tested them, please let me know. Examples are included in each part, please go through them first before you start your own piece.

### Workload Type
  - [Training](https://github.com/AaspiralMoon/WiSeR_Cluster/tree/master/training)
  - [Distributed Training](https://github.com/AaspiralMoon/WiSeR_Cluster/tree/master/distributed_training):fire:
  
### Other Functionalities (under test)
  - Deploy with priority
  - Resource monitoring
</details>

<details>
  <summary>Commands:candy:</summary>
  
Frequently used commands (linux, docker, kubernetes) are listed as follows. Please check this part first when you have trouble.

### Linux Commands

- sudo no password
  
```python
$ sudo visudo
$ [user] ALL=(ALL) NOPASSWD: ALL
# ctrl+x to save and exit
```

- Docker commands without sudo

```python
$ sudo groupadd docker
$ sudo usermod -aG docker [user]
$ newgrp docker
```
  
### Docker Commands
 
- Build a docker image from dockerfile
  
```python
$ sudo docker build -f Dockerfile -t name:tag .
# example: sudo docker build -f Dockerfile -t yolov3:v1 .
```

- Create a container based on docker image
  
```python
$ sudo docker run --gpus all --network host --ipc host --name yolov3 -v local_path:container_path -it name:tag
# example: sudo docker run --gpus all --network host --ipc host --name yolov3 -v /home/renjie/projects/datasets:/usr/src/app/datasets -it yolov3:v1
# --gpus all enable gpu access in container
# --network host enable container to use local network
# --ipc host enable container to share memory with host system
# -v mount local directory to container, usually mount dataset to container
# -it run an interactive instance
```

- Bash into container
  
```python
$ sudo docker exec -it container_name /bin/bash 
# example: sudo docker exec -it yolov3 /bin/bash
```
 
- Check existing docker images
  
```python
$ sudo docker images
# if you have run the "docker commands without sudo" above, "sudo" can be omitted here.
```

- Delete a docker image
  
```python
$ sudo docker rmi image_id
```

- Delete all unused images
  
```python
$ sudo docker image prune --all
```

- Check running containers
  
```python
$ sudo docker ps
# run sudo docker ps -a to check all containers
```

- Stop a running container
  
```python
$ sudo docker stop container_name/container_id
```
  
- Delete a stopped container
  
```python
$ sudo docker rm container_name
```

- Delete all stopped containers
  
```python
$ sudo docker container prune
```
### Kubernetes Commands (run on master node)
  
- Check all nodes in the cluster
  
```python
$ kubectl get nodes -o wide
```
  
- Check running pods in the cluster
  
```python
$ kubectl get pods -o wide
# run kubectl get pods -A to check all pods
```

- Check running services in the cluster
  
```python
$ kubectl get services -o wide
# run kubectl get services -A to check all services
```
  
- Delete a pod
  
```python
$ kubectl delete pods pod_name
# run kubectl delete pods pod_name --grace-period=0 --force to forcefully delete a pod (run this command to delete pods stuck in "terminating" status
```
  
- Create and delete an deployment (most frequently used commands)
  
```python
$ kubectl apply -f deployment.yaml
$ kubectl delete -f deployment.yaml
```

- Check the logs information of a pod
  
```python
$ kubectl logs pod_name
```

- Check the description of a pod
  
```python
$ kubectl describe pod pod_name
```
</details>
