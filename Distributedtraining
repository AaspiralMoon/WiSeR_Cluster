# Distributed training code
```python
# On master machine 0
$ python -m torch.distributed.launch --nproc_per_node G --nnodes N --node_rank 0 --master_addr "192.168.1.1" --master_port 1234 train.py --batch-size 64 --data coco.yaml --cfg yolov5s.yaml --weights ''

# On machine R
$ python -m torch.distributed.launch --nproc_per_node G --nnodes N --node_rank R --master_addr "192.168.1.1" --master_port 1234 train.py --batch-size 64 --data coco.yaml --cfg yolov5s.yaml --weights ''

```
