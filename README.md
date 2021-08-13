# Caffe Tools
 > Based On：https://gitter.im/BVLC/caffe
 >
 > Ubuntu 14.04.1  (gcc version 4.8.4)

## 工具使用

```shell
source env.sh
cd tools
./caffe.bin 
caffe.bin: command line brew
usage: caffe <command> <args>

commands:
  train           train or finetune a model
  test            score a model
  device_query    show GPU diagnostic information
  time            benchmark model execution time

  Flags from tools/caffe.cpp:
    -gpu (Optional; run in GPU mode on given device IDs separated by ','.Use
      '-gpu all' to run on all available GPUs. The effective training batch
      size is multiplied by the number of devices.) type: string default: ""
    -iterations (The number of iterations to run.) type: int32 default: 50
    -level (Optional; network level.) type: int32 default: 0
    -model (The model definition protocol buffer text file.) type: string
      default: ""
    -phase (Optional; network phase (TRAIN or TEST). Only used for 'time'.)
      type: string default: ""
    -sighup_effect (Optional; action to take when a SIGHUP signal is received:
      snapshot, stop or none.) type: string default: "snapshot"
    -sigint_effect (Optional; action to take when a SIGINT signal is received:
      snapshot, stop or none.) type: string default: "stop"
    -snapshot (Optional; the snapshot solver state to resume training.)
      type: string default: ""
    -solver (The solver definition protocol buffer text file.) type: string
      default: ""
    -stage (Optional; network stages (not to be confused with phase), separated
      by ','.) type: string default: ""
    -weights (Optional; the pretrained weights to initialize finetuning,
      separated by ','. Cannot be set simultaneously with snapshot.)
      type: string default: ""
```

## 工具编译 （CPU Only）

### 1.依赖库
```shell
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler -y
sudo apt-get install --no-install-recommends libboost-all-dev -y
sudo apt-get install libatlas-base-dev -y
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev -y
```

### 2.编译配置
> caffe文件夹中默认含有一个示例Makefile，只要去复制修改这个文件就可以,
然后打开Makefile.config进行修改，如果没有GPU，所以使用CPU-ONLY模式。反注释掉CPU_ONLY := 1

```shell
cp Makefile.config.example Makefile.config
make all
make test
make runtest
```

### 3.编译问题
1. 软件包 libboost-mpi-python-dev 尚未配置等错误

   ```c
   dpkg: error processing package libboost-mpi-python1.54-dev (--configure):
    依赖关系问题 - 仍未被配置
   dpkg: dependency problems prevent configuration of libboost-mpi-python-dev:
    libboost-mpi-python-dev 依赖于 libboost-mpi-python1.54-dev；然而：
     软件包 libboost-mpi-python1.54-dev 尚未配置。
   
   dpkg: error processing package libboost-mpi-python-dev (--configure):
    依赖关系问题 - 仍未被配置
   dpkg: dependency problems prevent configuration of libboost-all-dev:
    libboost-all-dev 依赖于 libboost-mpi-python-dev；然而：
     软件包 libboost-mpi-python-dev 尚未配置。
   
   dpkg: error processing package libboost-all-dev (--configure):
    依赖关系问题 - 仍未被配置
   在处理时有错误发生：
    libboost-mpi-python1.54.0
    libboost-mpi-python1.54-dev
    libboost-mpi-python-dev
    libboost-all-dev
   E: Sub-process /usr/bin/dpkg returned an error code (1)
   ```

   

   解决方法如下：

```shell
sudo mv /var/lib/dpkg/info /var/lib/dpkg/info_old
sudo mkdir /var/lib/dpkg/info
sudo apt-get update
sudo apt-get -f install
sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info_old
sudo rm -rf /var/lib/dpkg/info
sudo mv /var/lib/dpkg/info_old /var/lib/dpkg/info
```

