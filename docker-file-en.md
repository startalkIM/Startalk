[Content](README-en.md#Contents)

# Introduction

Before the privatization deployment, effective trial is necessary to understand how this system works, what services are required, and what functions are available.

so it will make you more aware of the functional scope and performance indicators of the platform.

There are 2 trials are recommended:

* [Sign up with public cloud](https://www.startalk.im)
* using docker


## deploying with docker

### docker download & installation
* [Windows version](https://docs.docker.com/docker-for-windows/install/)
* [Mac version](https://docs.docker.com/docker-for-mac/install/)
* [Ubuntu version](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
* [Centos version](https://docs.docker.com/install/linux/docker-ce/centos/)
* [Debian version](https://docs.docker.com/install/linux/docker-ce/debian/)
* [Other Linux distributions](https://docs.docker.com/install/linux/docker-ce/binaries/)

# Document below is deprecated, please review the latest docker-compose at: https://github.com/startalkIM/startalk-docker-compose  

### download file
+ Get the resource [file](https://i.startalk.im/pubapi/soft/download/startalk_docker.zip) and extract it:

```
wget https://i.startalk.im/pubapi/soft/download/startalk_docker.zip
```
+ Unzip the file

```
unzip -x startalk_docker.zip
```

you get:
```
-rw-r--r--. 1 root root 1.3K Dec 10 2019 Dockerfile
drwxr-xr-x. 2 root root 33 Dec 11 2019 images
-rw -------. 1 root root 22M December 10 2019 permfile.zip
-rw-r--r--. 1 root root 1.9K Dec 11 2019 README.md
-rw-r--r--. 1 root root 93K Dec 10 2019 success.png
```

Unzip it later:
```
unzip -x permfile.zip
```

you get:
```
-rw-r--r--. 1 root root 1.3K Dec 10 2019 Dockerfile
drwxr-xr-x. 2 root root 33 Dec 11 2019 images
drwxr-xr-x. 6 root root 57 Dec 9 2019 permfile
-rw -------. 1 root root 22M December 10 2019 permfile.zip
-rw-r--r--. 1 root root 1.9K Dec 11 2019 README.md
-rw-r--r--. 1 root root 93K Dec 10 2019 success.png
```

+ Load image with docker
```
docker load <images / startalk_docker.tar
```

+ Create Volume
> Why create a volume?
>
> In simple terms, Docker environment cannot save data under virtual machine. You need to use your host machine to save these files, such as logs, such as pictures, such as chat records.

```
docker volume create startalkpgdata
```

+ Get docker image id
```
docker images

copy the IMAGE ID corresponding to the column with the largest size in the result.
```

+ Boot image

> Need to prepare:
> * IP of your own host (hosturl below)
> * Docker image id obtained in the previous step (the image id below)

* liunx users use:

```
Assume:
permfile file's full path is /home/xxx/permfile
Host IP is 192.168.0.1
docker image id is 1e977139e6ba

Then run directly:
docker run -v /home/xxx/permfile:/startalk/permfile -p 8080: 8080 -p 5202: 5202 -e hosturl = "192.168.0.1" 1e977139e6ba
```

**Note that the current version of Docker must use the full path to load the permfile, otherwise it may fail to start with a high probability**

* For Windows users:
```

Assume:
Host IP is 192.168.0.1
docker image id is 1e977139e6ba

Then run directly:
docker run -v permfile: / startalk / permfile -v startalkpgdata: / startalk / data -p 8080: 8080 -p 5202: 5202 -e hosturl = "192.168.0.1" 1e977139e6ba
```

note:

* docker run with -v parameter, there is the absolute path of the persistent file on the host machine followed. If you unzip permfile.zip to /home/startalk/permfile then -v /startalk/ permfile: /startalk/permfile. The path after the colon does not need to be changed.

* docker run with the -p parameter to configure the port mapping between the host and the container. In this example, the host needs to be used to expose two ports: 8080 and 5202.

* hosturl is the host's ip, which is passed as a parameter to the container. If you configure the intranet IP, the whole set of IM can only be used on the intranet. Otherwise, if you are using a public IP, the external network can also be used.

* Docker version: 18.09.2 is used for build image, to avoid problems caused by tools, >= 18.09 of docker version is recommended 


Here is the original text [Address] (https://github.com/startalkIM/startalk-docker)

[Content](README-en.md#Contents)
