# Prepare your operating system (OS)
- Ubuntu20.04 LTS

# Install GPU driver
```
$ sudo apt install nvidia-driver-510
# Run nvidia-smi to check if the driver is working.
```

# Install cuda-11.1 and cudnn-8.0.4
```
# Download cuda and cudnn
$ cd /home/downloads
$ wget 
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

# After configuring cuda and cudnn, run nvcc --version to check if cuda is working well.
```

# Install Docker
```
$ sudo apt update
$ sudo apt install docker.io
```

# Configure Docker
- sudo nano /etc/docker/daemon.json
- add "exec-opts": ["native.cgroupdriver=systemd"]

# Enable GPU in Docker
- curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
- distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
- curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
- sudo apt-get update
- sudo apt-get install -y nvidia-docker2
- sudo systemctl restart docker

# Install kubernetes
- apt-cache policy kubectl  (search for package versions)
- sudo apt-get update
- sudo apt-get install -y apt-transport-https ca-certificates curl
- sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
- echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
- sudo apt-get update
- sudo apt-get install -y kubelet=1.22.11-00  kubeadm=1.22.11-00 kubectl=1.22.11-00
- sudo apt-mark hold kubelet kubeadm kubectl

# Configure kubernetes
- sudo swapoff -a
- sudo nano /etc/fstab and comment the "swap" line.
- sudo nano /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
- Add Environment=”cgroup-driver=systemd/cgroup-driver=cgroupfs” after the last “Environment Variable”.

# Enable GPU in kubernetes: [related webpage](https://docs.nvidia.com/datacenter/cloud-native/kubernetes/install-k8s.html)
- In worker nodes:
	- sudo nano /etc/docker/daemon.json
	- add "default-runtime": "nvidia", in the first line (DO NOT forget comma).
	- sudo systemctl restart docker

- In master nodes:
	- curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 \
   && chmod 700 get_helm.sh \
   && ./get_helm.sh
   	- helm repo add nvdp https://nvidia.github.io/k8s-device-plugin \
   && helm repo update
   	- helm install --generate-name nvdp/nvidia-device-plugin --namespace kube-system
