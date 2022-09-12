# Prepare your operating system (OS)
- Ubuntu20.04 LTS

# Install GPU driver
- sudo apt install nvidia-driver-510
- Run nvidia-smi to check if the driver is working.

# Install cuda and cudnn
- cuda 11.1
- cudnn 8.0.4

# Install Docker
- sudo apt update
- sudo apt install docker.io

# Configure Docker
- sudo nano /etc/docker/daemon.json
- add "exec-opts": ["native.cgroupdriver=systemd"]

