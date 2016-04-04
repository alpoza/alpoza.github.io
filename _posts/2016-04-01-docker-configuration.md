---
layout: post
title: "docker-configuration"
published: true
Category: dev
Tags: git
Authors: Alberto Poza
Summary: Configurando docker
---
**Configurando docker**

## Antecedentes
- Tenemos un servidor con Docker instalado
- El directorio de trabajo no tiene mucho espacio en disco
- Los resgistros privados no fucncionan
- El servidor no tiene salida directa con Internet

##
- find out where the service file is located: 
sudo systemctl status docker | grep Loaded
- Cambiar el directorio de trabajo de Docker (images and containers) en el fichero del servicio Docker
[Service]
Type=notify
ExecStart=/usr/bin/docker daemon -H fd:// -g /data/docker -p /var/run/docker.pid
- recargar el daemon: sudo systemctl daemon-reload
- reestart el servicio sudo systemctl restart docker
