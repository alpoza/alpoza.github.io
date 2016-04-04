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
- El proxy tiene autenticación NTLM.

## Pasos
- Montar un proxy que enrute todas las peticiones al proxy corporativo en nuestra máquina y que admita NTLM
- Crear un tunnel Remoto en el servidor para que conecte el servidor al proxy de nuestra máquina

## Herramientas candidatas
- Burp suite
- Fiddler
- cntlm

## Problemas
- Burp suite conseguí montarlo con la versión enterprise, pero el rendimiento era muy pobre y tenía problemas con algunos sites. Con https los certificados eran los de Burp, con lo que había problemas con ciertos clientes que había que parametrizar adecuadamente. Algunos como Yum, git, etc, que utilizan por debajo curl, wget, etc. hacían cosas raras.
- Fiddler lo monté para probar despues de encontrar la solución con cntml. Aparentemente funcionaba bien pero no realicé demasiadas pruebas.
- CNTML !!!! ¿como no lo descubría antes? Rápido, sin problemas y fucniona a la perfección con la combinación de proxy, authenticación, etc. de la que disponía. Además con versión Linux y Windows. La configuración sencilla y el rendimiento perfecto.

## Más problemas
- Para descargar e instalar paquetes y utilizarlo como cliente perfecto. Los primeros problemas los obtuve utilizando Docker. El proxy no se veía desde dentro de los contenedores. El problema es que el tunnel por defecto escuha sólo en localhost, aun indicando en la conexión que quieres que escuche en todas las interfaces 0.0.0.0:8088. Para solucionarlo hay que configurar el servicio sshd añadiendo el parámetro que indica que escuche en el puerto que indique el cliente. Ver esta respuesta de superuser.com: https://superuser.com/questions/588591/how-to-make-ssh-tunnel-open-to-public 
  
  Note that if you use OpenSSH sshd server, the server's GatewayPorts option needs to be enabled (set to yes or clientspecified) for this to work (check file /etc/ssh/sshd_config on the server). Otherwise (default value for this option is no), the server will always force the port to be bound on the loopback interface only.

Despues de reiniciar el servicio ya podemos arrancar nuestro tunel indicando que queremos que escuche en todas las interfaces: ssh -R 0.0.0.0:8088:localhost:8089 -N root@example.com
esto es: si hacemos un export http_proxy=http://example.com:8088 todos los servicios que utilicen esa variable para establecer el proxy enviaran las peticiones http mediante el tunel a nuestra máquina local en el puerto 8089, en el que deberá estar escuchando nuestro proxy (cntlm) que a su vez remete las peticiones al proxy corporativo.
