# Distributed training code
```python
# On master machine 0
$ python -m torch.distributed.launch --nproc_per_node 1 --nnodes N --node_rank 0 --master_addr "192.168.11.17" --master_port 8085 train.py --img 640 --batch 16 --epochs 5 --data coco128.yaml --weights yolov3.pt

# On machine R
$ python -m torch.distributed.launch --nproc_per_node 1 --nnodes 2 --node_rank 0 --master_addr "192.168.11.17" --master_port 8085 train.py --img 640 --batch 16 --epochs 5 --data coco128.yaml --weights yolov3.pt

```
