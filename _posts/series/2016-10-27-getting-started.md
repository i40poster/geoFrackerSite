---
layout: post
title: "<b>GeoFracker Studio Steup(Part 1)</b> Let's start!"
excerpt: "Exploring some Sao Paulo data with geoFracker"
tags:
  - Maps
  - SAO PAULO
  - R
  - geoFracker
image:
  #feature: so-simple-sample-image-7.jpg
  #credit: WeGraphics
  #creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/

permalink: /RStudioSetupV2
comments: true
share: true
categories: [series]
date: 2016-09-22
modified: 2016-10-27
author: cesar
---

* Table of Contents
{:toc}

# Overview

This guide will cover how to use the project [geoFracker](https://github.com/i40poster/geoFracker) to help you to start using RStudio inside of a docker image to avoid too much struggle to install the required libs.

[geoFracker](https://github.com/i40poster/geoFracker) is a extended version of [rocker-org/hadleyverse](https://github.com/rocker-org/hadleyverse) RStudio distribution of docker with some custom libs to help you to process some Sao Paulo data offered at [GeoSampa](http://geosampa.prefeitura.sp.gov.br/PaginasPublicas/_SBC.aspx#)

&nbsp;
&nbsp;

<figure>
	<a href="{{site.url}}/images/geoFrackerIntro.svg"><img src="{{site.url}}/images/geoFrackerIntro.svg" alt="image" style="width:100%"></a>
	<figcaption>GeoFracker Overview</figcaption>
</figure>

>
> [Docker](https://docker.com) is easy way to pack software to easy distribution of softwares that works as micro services, as for example RStudio. Docker works well on windows, mac or linux.



# Initial Setup - Creating our workspace

## Pre-Requirements

The next steps will require you to have docker.

## what is docker?
<div class="flex-video">
  <iframe src="//www.youtube.com/embed/aLipr7tTuA4" frameborder="0"></iframe>
</div>


For details on how to install and setup docker check out this page: <https://docs.docker.com/>

For Windows and Mac check out: <https://www.docker.com/products/docker-toolbox>


## Running your workspace

Running your workspace: followw the steps described [here](https://hub.docker.com/r/it4poster/geofracker/)

```bash
docker pull it4poster/geofracker
docker run -d -p 8787:8787 -p 6311:6311 it4poster/geofracker
```

Open your browser at <http://**dockerip**:8787/>

Login:

| user | password |
|:--:|:--:|
| rstudio | rstudio|
{: .table}

> Example of RStudio url running on dockers.
>
> Running on docker machine: <http://192.168.99.100:8787>
>
> Running on localhost(linux): <http://localhost:8787>



At your browser rstudio:

```R
source('/home/rstudio/setupLibs.R')
#wait the installs to be completed
```



# Let's explore some data

Exploring a basic São Paulo Data:

* geofracker.downloadAndUnzipShp(URL) -> Allows you to download a shapefile for processing.

```R

Edificacao = geofracker.downloadAndUnzipShp(saopaulo.data.edificacaoList[1])

Edificacao1 <- readOGR(dsn=Edificacao$dir[1], layer=Edificacao$shapeclass[1])
plot(Edificacao1)
#centers of edifications
pointsEdificacao <- rgeos::gCentroid(Edificacao1, byid=TRUE)
proj4string(pointsEdificacao) <- Edificacao1@proj4string
plot(pointsEdificacao)


```
> **saopaulo.data.edificacaoList**: It is list of files available already embeed on this studio version to help you to explore the data. e.g: saopaulo.data.edificacaoList[1] is the "5CSHP_edificacao_AGUA_RASA" file.

# Exploring the Center of Mass

**Center of Mass** is the center of the polygon(in this case an edification). We will use this property later to do some cool calculation.

```R
pointsEdificacao <- rgeos::gCentroid(Edificacao1, byid=TRUE)

proj4string(pointsEdificacao) <- Edificacao1@proj4string

plot(pointsEdificacao)
```


# Demo

<iframe width='970' height='546'
src="https://www.youtube.com/embed/kz_DIhT8kZc">
</iframe>


# Next Steps
For a more complete setup  check [this page]({{site.url}}/RStudioSetupV2-pt2) where you can enable ftp access to the `geofracker` container.
