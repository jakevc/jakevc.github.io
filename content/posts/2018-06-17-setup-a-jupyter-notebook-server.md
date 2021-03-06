---
title: Setup a jupyter notebook server
author: Jake VanCampen
date: '2018-06-17'
slug: setup-a-jupyter-notebook-server
tags:
  - jupyter
  - server
  - ssh
category: "jupyter"
image: "https://source.unsplash.com/gM7LYJYrBSw"
---

# Setup a jupyter notebook server

This document is a quick how to on running Jupyter notebook/lab a remote server. 


## Setup

Make sure you have an up to date install of anaconda or miniconda. If you need to, install miniconda3 in your home directory:

```
cd ~
wget https://repo.continuum.io/miniconda/Miniconda3-4.5.4-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

Follow the commands to install using defaults, then restart your terminal session. When you update or install programs using this version of conda, their executable is stored in: /home/<user>/miniconda3/bin. To allow these programs to be executable from the command line, you need to prepend this location to your PATH environment variable. Add the following line to your ~/.bashrc file to see minconda3 in your PATH:

```
# add miniconda3 bin to PATH
export PATH="$HOME/miniconda3/bin:$PATH"
```
Then install Jupyter using conda: 

```
conda install jupyter
```

This will prompt you to install jupyter and it's dependancies in your current environment. 

## Setup Port Forward

To run "jupyter notebook" on a computational server and edit jupyter notebooks that live in your local file system/ workspace, you can setup a port forward by ssh tunnel. To do this, you assign a port from your personal computer to listen on a remote port (whatever computational server you run 'jupyter notebook' from).  This can be accomplished with the following setup. 

Open a terminal session on your personal computer and assign the port forward (remote port 8889 to local port 8888), using your login credentials, for example: john777@remote_host. Any two valid ports could be used in this setup. 

ssh -N -f -L localhost:8888:localhost:8889 remote_user@remote_host
Then, ssh into <remote_host> using your linux account credentials, and run:

jupyter notebook --no-browser --port-8889
This sends a jupyter notebook session to port 8889 on the server, and does not attempt to automatically open a browser. 

## Go listen

You can now access the running remote jupyter notebook by typing "localhost:8888" in the address bar of your web browser. It will ask you for a token, which you can copy and paste from the web-address that appears when you run the "jupyter notebook" command for example:

jupyter notebook --no-browser --port=8889
[I 13:25:56.032 NotebookApp] Serving notebooks from local directory: /n/path/to/cool/notebook
[I 13:25:56.032 NotebookApp] 0 active kernels
[I 13:25:56.032 NotebookApp] The Jupyter Notebook is running at:
[I 13:25:56.033 NotebookApp] http://localhost:8889/?token=4ff75275e4cf53b235bade5a675756e0a913d98876eedcb8
[I 13:25:56.033 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 13:25:56.034 NotebookApp]
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8889/?token=4ff75275e4cf53b235bade5a675756e0a913d98876eedcb8&token=4ff75275e4cf53b235bade5a675756e0a913d98876eedcb8
You would copy and paste the hash that appears after the "token=" into the box that the browser provides.

## Jupyter Lab 
The same steps above can be used to access the new JupyterLab on a remote server. With the same port forward running, spin up a JupyterLab using the following command: 

`jupyter lab --no-browser --port=8889`

If you need to disable the port-forward this at some point, you can kill the process ID, which you can look for using the following command: 

`ps aux | grep localhost:8889`

## Jupyter Config

Once this is setup you can configure a password for easier access. Exit the browser and the quit the jupyter notebook on the remote server, then from the home directory of your linux account execute: 

```
jupyter notebook password
```

This will prompt you to enter and verify a password that you can use instead of copy and pasting the token into your browser each time. This is done in the back end by storing a hash of your password in the file: ~/.jupyter/jupyter_notebook_config.json. 

For more information on the topic consult: [Securing a notebook server](http://jupyter-notebook.readthedocs.io/en/stable/public_server.html)

