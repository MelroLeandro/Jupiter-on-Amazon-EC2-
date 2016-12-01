## TensorFlow with GPU in Anaconda env [Ubuntu 16.04 + CUDA 7.5 + cuDNN]

Install Driver from Xenial + ppa:
```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt-get update
$ sudo ubuntu-drivers autoinstall
$ sudo reboot
```
> Software & Updates > Additional Drivers
Change from nvidia-364 (open source) to "Using NVIDIA - version 361.42 from nvidia-361 (proprietary)

Download the CUDA toolkit from
https://developer.nvidia.com/cuda-toolkit
The toolkit runfile uses an older driver than we've installed above. So select "No" to driver.
```
$ chmod 755 cuda_7.5.18_linux.run
$ sudo ./cuda_7.5.18_linux.run --override
```
Download cuDNN 4 from
https://developer.nvidia.com/rdp/cudnn-download
Extract. Rename the cuda directory as cudnn. We'll keep cudnn separate from cuda.
```
$ mv cuda cudnn
$ sudo cp -r cudnn /usr/local
```

Create symlinks in /usr/local/cuda/lib64 to /usr/local/cudnn/lib64
```
$ sudo ln -s /usr/local/cuda/lib64/libcudnn.so /usr/local/cudnn/lib64/libcudnn.so
$ sudo ln -s /usr/local/cuda/lib64/libcudnn.so.4 /usr/local/cudnn/lib64/libcudnn.so.4
$ sudo ln -s /usr/local/cuda/lib64/libcudnn.so.4.0.7 /usr/local/cudnn/lib64/libcudnn.so.4.0.7
$ sudo ln -s /usr/local/cuda/lib64/libcudnn_static.a /usr/local/cudnn/lib64/libcudnn_static.a
```
Set up ~/.bashrc
```
$ sudo nano ~/.bashrc
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cudnn/lib64
export CUDA_HOME=/usr/local/cuda
export PATH=/usr/local/cuda/bin:$PATH
```
Environment Variables:
```
$ sudo nano /etc/profile.d/cuda.sh
export PATH=$PATH:/usr/local/cuda/bin

$ sudo nano /etc/ld.so.conf.d/cuda.conf
/usr/local/cuda/lib64

$ sudo nano /etc/ld.so.conf.d/cudnn.conf
/usr/local/cudnn/lib64

$ sudo ldconfig
$ source ~/.bashrc
```
Force it to work with gcc 5:
https://www.pugetsystems.com/labs/articles/NVIDIA-CUDA-with-Ubuntu-16-04-beta-on-a-laptop-if-you-just-cannot-wait-775/
```
$ sudo nano /usr/local/cuda/include/host_config.h
line: 115 comment out error
//#error -- unsupported GNU version! gcc versions later than 4.9 are not supported!
```
Verify your driver & CUDA version and directory
```
$ nvidia-smi
$ nvcc -V
$ which nvcc
```
Download Anaconda - Either Python 2.7 or 3.5
https://www.continuum.io/downloads
```
$ bash Anaconda2-4.0.0-Linux-x86_64.sh
```
Create a tensorflow environment using Python 3.5:
```
$ conda create -n tensorflow python=3.5
```
Download tensorflow & rename the whl:
```
$ wget https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.8.0-cp34-cp34m-linux_x86_64.whl
$ mv tensorflow-0.8.0-cp34-cp34m-linux_x86_64.whl tensorflow-0.8.0-cp35-none-linux_x86_64.whl
```
Start the "tensorflow" env
```
$ source activate tensorflow
(tensorflow) $ pip install --ignore-installed --upgrade tensorflow-0.8.0-cp35-none-linux_x86_64.whl
```
To exit the env
```
(tensorflow) $ source deactivate 
```
##
