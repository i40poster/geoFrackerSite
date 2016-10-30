---
layout: post
title: "<b>GeoFracker Studio Steup(Part 2)</b> Improving your workspace"
excerpt: "Adding a file storage and FTP access"
tags:
  - Docker
  - R
  - geoFracker
image:
  #feature: so-simple-sample-image-7.jpg
  #credit: WeGraphics
  #creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/

permalink: /RStudioSetupV2-pt2
comments: true
share: true
categories: [series]
date: 2016-10-30
modified: 2016-10-30
author: cesar
---

* Table of Contents
{:toc}

# Overview

A suggested way to use this studio to simplify file acess it is the following:
<figure>
	<a href="{{site.url}}/images/rGeoFrackerWorkspace.svg"><img src="{{site.url}}/images/rGeoFrackerWorkspace.svg" alt="image" style="width:100%"></a>
	<figcaption>GeoFracker Docker Topology</figcaption>
</figure>


# Setting up the workspace

1st time - environment setup

```bash
#Data storage volume
docker volume create --name ws_not_remove
#create the RStudio Docker
docker run -d --name r_geofracker -v ws_not_remove:/home/rstudio/workspace -p 8787:8787 -p 6311:6311 it4poster/geofracker
docker run -d -v ws_not_remove:/srv/ftp --name ftp_geofracker -p 21:21 -p 30000-30009:30000-30009 -e USER=ftpuser1 -e PASSWORD=qaz123 it4poster/pure-ftpd-sample

```

# Updating environment

if you need to update the environment just run:

```bash
#Stop containers
docker stop r_geofracker
docker stop ftp_geofracker
#Remove old containers(be aware this will remove everything outside of the folder /home/rstudio/workspace)
docker rm r_geofracker
docker rm ftp_geofracker

#update images
docker pull it4poster/geofracker
docker pull it4poster/pure-ftpd-sample

#Start new containers - fresh new workspace
docker run -d --name r_geofracker -v ws_not_remove:/home/rstudio/workspace -p 8787:8787 -p 6311:6311 it4poster/geofracker
docker run -d -v ws_not_remove:/srv/ftp --name ftp_geofracker -p 21:21 -p 30000-30009:30000-30009 -e USER=ftpuser1 -e PASSWORD=qaz123 it4poster/pure-ftpd-sample

```

> We will explore `docker volumes` on this tutorial, a very simple way to persist and share data between containers.

After you setup the volume once, you can check if it still there with:

```bash

docker volume inspect ws_not_remove

```

> Once the container it is created, you can run new containers pointing to the same place, and the data inside the shared folder will continue to be there. Inside the `r_geofracker` container, the folder `/home/rstudio/workspace` will be stored inisde `ws_not_remove` and not at `r_geofracker`.

# Summary

This guide is meant ot help with the following
- Enable ftp access to your files inse `r_geofracker`, to access use ftp://`docker_host_ip`, and all files of the folder `/home/rstudio/workspace` will be there;
- Enable persitence between `r_geofracker` image updates;
- This setup works on local setup, running docker in your machine;
- This setup works on remote setup, running docker on a machine in your LAN;
- This setup works on cloud setup, running docker in the cloud if you desire to share this studio with someone else, `on this scenarios I would advice to change default passwords`.


# Expected result

Running images:
```bash
docker ps
```

```text
CONTAINER ID        IMAGE                        COMMAND                   CREATED             STATUS              PORTS                                                      NAMES
58894a4f6a81        it4poster/pure-ftpd-sample   "/bin/sh -c 'echo \"$P"   7 minutes ago       Up 7 minutes        0.0.0.0:21->21/tcp, 0.0.0.0:30000-30009->30000-30009/tcp   ftp_geofracker
04d4f404c235        it4poster/geofracker         "/init"                   7 minutes ago       Up 7 minutes        0.0.0.0:6311->6311/tcp, 1410/tcp, 0.0.0.0:8787->8787/tcp   r_geofracker
```

Data Volumes:

```bash
docker volume inspect ws_not_remove
```

```text
[
    {
        "Name": "ws_not_remove",
        "Driver": "local",
        "Mountpoint": "/mnt/sda1/var/lib/docker/volumes/ws_not_remove/_data",
        "Labels": {}
    }
]
```

# Tips

To access the files via FTP, you can use tools like:
- <https://filezilla-project.org/> - Read/Write client
- <mozilla.org - Download Firefox - Firefox is Faster Than Ever‎> - Read-Only client
- and others...

# References

<https://docs.docker.com/engine/reference/commandline/volume_create/>
