DMLC Tracker
============
Job submission and tracking script for DMLC. To submit your job to cluster.
Use the following command

```bash
cd /usr/local/mxnet/lib64/example/image-classification/ && zip -r libmnist common/ symbols/
mv libmnist.zip /home/hadoop/

mxnet-submit \
    --jobname distMnist \
    --cluster yarn \
    --hadoop-home ${HADOOP_HOME:-/usr/lib/hadoop} \
    --hadoop-hdfs-home ${HADOOP_HDFS_HOME:-/usr/lib/hadoop-hdfs} \
    --ps-verbose 2 \
    --num-workers 2 \
    --num-servers 2 \
    --worker-cores 1 \
    --server-cores 4 \
    --worker-memory 12g \
    --server-memory 5g \
    --log-level DEBUG \
    --auto-file-cache 1 \
    --archives libmnist.zip \
    python /usr/local/mxnet/lib64/example/image-classification/train_mnist.py \
        --network mlp \
        --num-epochs 10 \
        --gpus 0 \
        --kv-store dist_device_sync
```

DMLC job will start executors, each act as role of worker or server.
It works for both parameter server based jobs as well as rabit allreduce jobs.

Parameters
----------
To get full list of arguments:
```bash
mxnet-submit -h
```