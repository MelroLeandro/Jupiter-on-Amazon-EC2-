# Jupiter on Amazon EC2

Installing Jupiter on Amazon EC2 

##I - Anaconda install on EC2 (ubuntu instance)
  ```sh
  cd ~
  wget https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh
  bash Anaconda3-4.2.0-Linux-x86_64.sh -b
  echo 'PATH="/home/ubuntu/anaconda3/bin:$PATH"' >> .bashrc
  source ~/.bashrc
  ```

##II - jupyter setup
  ```sh
  conda update jupyter
  jupyter notebook --generate-config

  key=$(python -c "from notebook.auth import passwd; print(passwd())")

  cd ~
  mkdir certs
  cd certs
  certdir=$(pwd)
  openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout my.key -out my.pem

  cd ~
  sed -i "1 a\
  config = get_config()\\
  config.NotebookApp.certfile = u'$certdir/mycert.pem'\\
  config.NotebookApp.keyfile = u'$certdir/mycert.key'\\
  config.NotebookApp.ip = '*'\\
  config.NotebookApp.open_browser = False\\
  config.NotebookApp.password = u'$key'\\
  config.NotebookApp.port = 8889" .jupyter/jupyter_notebook_config.py
  ```
 
##III - Launch jupyter 
  ```sh
  cd ~
  mkdir notebook_root
  cd notebook_root
  jupyter notebook
  ```

# Installing TensorFlow on an AWS EC2 Instance with GPU Support

The following things are installed:

    Essentials
    Cuda Toolkit 7.0
    cuDNN Toolkit 6.5
    Bazel 0.1.4 (Java 8 is a dependency)
    TensorFlow 0.6

After launching your instance g2.2xlarge  using the Ubuntu Server 14, install the essentials :
```
  sudo apt-get update
  sudo apt-get upgrade
  sudo apt-get install -y build-essential git python-pip libfreetype6-dev libxft-dev libncurses-dev libopenblas-dev gfortran python-matplotlib libblas-dev liblapack-dev libatlas-base-dev python-dev python-pydot linux-headers-generic linux-image-extra-virtual unzip python-numpy swig python-pandas python-sklearn unzip wget pkg-config zip g++ zlib1g-dev
  sudo pip install -U pip
  ```
 TensorFlow requires installing CUDA Toolkit 7.0. To do this, run:
 ```
  wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1410/x86_64/cuda-repo-ubuntu1410_7.0-28_amd64.deb
  sudo dpkg -i cuda-repo-ubuntu1410_7.0-28_amd64.deb
  rm cuda-repo-ubuntu1410_7.0-28_amd64.deb
  sudo apt-get update
  sudo apt-get install -y cuda
```
# XGBoost on AWS

 ```
  sudo apt-get install make
  sudo apt-get update
  sudo apt-get install gcc
  sudo apt-get install g++
  sudo apt-get install git
  sudo git clone https://github.com/dmlc/xgboost
  cd xgboost
  ./build.sh
  cd python-package
  python setup.py install
  wget https://bootstrap.pypa.io/ez_setup.py -O - | sudo python
 ```
 
# Making an Amazon EBS Volume Available for Use

  To make an EBS volume available for use on Linux
  
 1. Connect to your instance using SSH. For more information, see Step 2: Connect to Your Instance.
 2. Use the lsblk command to view your available disk devices and their mount points (if applicable) to help you determine the correct device name to use.
 
 ```
  [ec2-user ~]$ lsblk
  NAME  MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
  xvdf  202:80   0  100G  0 disk
  xvda1 202:1    0    8G  0 disk /
  ```
 
 The output of lsblk removes the /dev/ prefix from full device paths. In this example, /dev/xvda1 is mounted as the root device (note the MOUNTPOINT is listed as /, the root of the Linux file system hierarchy), and /dev/xvdf is attached, but it has not been mounted yet.
 3. Determine whether you need to create a file system on the volume. Use the sudo file -s device command to list special information, such as file system type.
 '''
 
 '''
