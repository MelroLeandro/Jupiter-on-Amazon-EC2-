# Jupiter on Amazon EC2

Installing Jupiter on Amazon EC2 

##I - Anaconda install on EC2 (ubuntu instance)
  '''sh
  cd ~
  wget https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh
  bash Anaconda3-4.2.0-Linux-x86_64.sh -b
  echo 'PATH="/home/ubuntu/anaconda3/bin:$PATH"' >> .bashrc
  source ~/.bashrc
  '''

##II - jupyter setup
  '''sh
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
  '''
 
##III - Launch jupyter 
  '''sh
  cd ~
  mkdir notebook_root
  cd notebook_root
  jupyter notebook_root
  '''
