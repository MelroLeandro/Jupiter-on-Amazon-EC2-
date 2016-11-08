#JupyterHub

With JupyterHub you can create a multi-user Hub which spawns, manages, and proxies multiple instances of the single-user Jupyter notebook (IPython notebook) server.

JupyterHub provides single-user notebook servers to many users. For example, JupyterHub could serve notebooks to a class of students, a corporate workgroup, or a science research group.

by Project Jupyter

## Deploying JupyterHub on AWS

1. Start a new machine using an instance type (c4.2xlarge) and Ubuntu Trusty 14.04 (Security group is configured to have 22, 80 & 443 opened)

2. Create server directory 
```
  $ sudo mkdir /srv/jupyterhub; sudo chown -R ubuntu:ubuntu /srv/jupyterhub
```

3. Use letsencrypt to generate your keys for the server

    - Connect to your machine using
```
    $ ssh -i <path_to_your_private_key> ubuntu@<your_public_ip>
```
    - You will need git installed:
```
    $ sudo apt-get update

    $ sudo apt-get install git
```
    - Install letsencrypt
```
    $ git clone https://github.com/letsencrypt/letsencrypt
    $ cd letsencrypt
```
        Generate the keys
```
    $ ./letsencrypt-auto certonly --standalone -v -d <host.domain.name> v
        Store the keys 
        
        $ mkdir /srv/jupyterhub/ssl
        $ sudo cp /etc/letsencrypt/live/<host.domain.name>/fullchain.pem /etc/letsencrypt/live/<host.domain.name>/privkey.pem /srv/jupyterhub/ssl
        ```
