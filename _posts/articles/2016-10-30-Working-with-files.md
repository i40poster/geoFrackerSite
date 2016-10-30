---
layout: post
title: "GeoDocker - Working with files"
excerpt: "Docker, copying files around"
tags:
  - Docker
  - geofracker
image:
  #feature: so-simple-sample-image-7.jpg
  #credit: WeGraphics
  #creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
comments: true
share: true
categories: [articles]
date: 2016-10-30
author: cesar
---

Before we start, did you have followed the setup steps described at [here]({{site.url}}/RStudioSetupV2)


# Overview

On this article, you will be able to learn how to transfer files between your machine and a docker container.

Our container of example in this article is the `r_geofracker` which we created at [this guide]({{site.url}}/RStudioSetupV2-pt2).

<figure>
	<a href="{{site.url}}/images/rGeoFrackerDocker.svg"><img src="{{site.url}}/images/rGeoFrackerDocker.svg" alt="image" style="width:100%"></a>
	<figcaption>GeoFracker</figcaption>
</figure>

> There is two possible direction to move the files:
> - 1: from your machine to the container
> - 2: from the container to your machine.

# Copying files to the containers
If you want to copy a file from your machine to a container, you just need to run:

```bash
docker cp /file/path/filename <container_name>:/folder/on/container/filename

#eg:(our default container name is r_geofracker)
docker cp /var/teste.txt r_geofracker:/home/rstudio/teste.txt
```

This would copy the file `/var/teste.txt` from your machine to the path at `/home/rstudio/teste.txt` at the container running with the name `r_geofracker`.

# Copying files from containers

If you want to copy a file from an container to to your machine, you just need to run:

```bash
docker cp <container_name>:/folder/on/container/filename /file/path/filename

#eg:(our default container name is r_geofracker)
docker cp r_geofracker:/home/rstudio/testeBack.txt /var/testeBack.txt
```

This would copy the file `/home/rstudio/testeBack.txt` from your cointainer named `r_geofracker` to the file `/var/testeBack.txt` at your machine.

# Final thoughts

Now, you can move files from and to your container.

# References

<https://docs.docker.com/engine/reference/commandline/cp/>
