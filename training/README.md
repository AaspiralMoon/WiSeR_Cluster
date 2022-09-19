<h1 align="center">A Training Demo</h1>

### Download this repo
```python
$ git clone https://github.com/AaspiralMoon/WiSeR_Cluster/
# if you do not have git, install it by "sudo apt install git"
```

### Register an account of [DockerHub](https://hub.docker.com/)
```python
# After registration, login in your account on your computer
$ docker login -u=username -p=password
```

### Build a docker image from Dockerfile
```python
# check the path, make sure you are under "WiSeR_Cluster/training"
$ cd WiSeR_Cluster/training 
$ sudo docker build -f Dockerfile -t yolov3-training:v1 .  # it will take several mins
$ sudo docker images # check if the image has been built
```

### Tag the image and push it to your DockerHub
```python
$ sudo docker images # find the image_id
$ sudo docker tag image_id username/name:tag # example: sudo docker tag image_id renjie/yolov3-training:v1
$ sudo docker push username/name:tag # it will take several mins
```

### Create a deployment in the cluster
```python
# In deployment.yaml, you have to change the "image: user_name/name:tag" based on your setup.
# Or you can use my image, "mcxrj/yolov3-ddp-training:v6"
$ kubectl apply -f deployment.yaml  # run this command on the master node
$ kubectl get pods -o wide # check the pods running status and the scheduled nodes.
$ kubectl logs pod_name # use this command to check the training status
```

### Get the weight file from remote pods to local space
```python
$ kubectl cp pod_name:remote_path local_path
```
