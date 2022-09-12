# Prepare your operating system (OS)
- Ubuntu20.04 LTS

# Install GPU driver
- sudo apt install nvidia-driver-510
- Run nvidia-smi to check if the driver is working.

# Install cuda and cudnn
- cuda 11.1
- cudnn 8.0.4

# Configure cuda
- gedit source ~/.bashrc
- source ~/. bashrc
- export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
- export PATH=$PATH:/usr/local/cuda/bin
- export CUDA_HOME=/usr/local/cuda

# Configure cudnn
- sudo cp -a  include/. /usr/local/cuda/include/
- sudo cp -a lib64/. /usr/local/cuda/lib64/
- After configuring cuda and cudnn, run nvcc --version to check if cuda is working well.

# Install Docker
- sudo apt update
- sudo apt install docker.io

# Configure Docker
- sudo nano /etc/docker/daemon.json
- add "exec-opts": ["native.cgroupdriver=systemd"]

# Install kubernetes
- apt-cache policy kubectl  (search for package versions)
- sudo apt-get update
- sudo apt-get install -y apt-transport-https ca-certificates curl
- sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
- echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
- sudo apt-get update
- sudo apt-get install -y kubelet=1.22.11-00  kubeadm=1.22.11-00 kubectl=1.22.11-00
- sudo apt-mark hold kubelet kubeadm kubectl
