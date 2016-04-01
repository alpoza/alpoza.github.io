---
layout: post
title: "proxying_the_proxy_or_die"
published: true
Category: dev
Tags: git
Authors: Alberto Poza
Summary: Configurando el proxy para pasar por el proxy
---
**Configurando el proxy para pasar por el proxy**

## Antecedentes
- Nos dan un servidor de desarrollo sin conexión a internet y con el proxy corporativo capada a nivel de Firewall.
- El servidor viene con servicio ssh.
- Nuestro portatil tiene configurado el proxy corporativo pero no tiene permisos para abrir puertos entrantes.

## Pasos
- Montar un proxy que enrute todas las peticiones al proxy corporativo en nuestra máquina
- Crear un tunnel Remoto en el servidor para que conecte el servidor al proxy de nuestra máquina

## Problemas

