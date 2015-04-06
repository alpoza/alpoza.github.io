---
layout: post
title: "springbootling REST web services"
published: true
Category: dev
Tags: REST, spring boot
Authors: Alberto Poza
Summary: Como utilizar spring boot para crear servicios REST
---
**Creando 'BootyFull' spring boot services sin dolor**

## Use Tomcat 7 or another 3.0 compliant application server
Según [Embedded Servlet containers] (http://docs.spring.io/spring-boot/docs/current/reference/html/howto-embedded-servlet-containers.html), Spring boot utiliza Tomcat 8 por defecto como embedded container. Si necesitas usar Jetty o Undertow tienes que configurarlos explicitamente según la guía. Si además necesitas desplegar el servicio en Tomcat 7 (porque estés usando Java 1.6 por ejemplo) o WebSphere Liberty Profile (WPL para los amigos) porque al final vas a desplegar en un WAS, si estás utilizando starters POMs and parents es tan sencillo como añadir 

  \<tomcat.version>7.0.59</tomcat.version> 
  
en los properties de tu __pom.xml__

Para que te funcione dentro de eclipse, quizá tengas que usar el [siguiente truco] (http://stackoverflow.com/questions/27447077/how-to-configure-spring-boot-1-2-0-for-servlet-3-0-and-have-m2e-set-eclipse-face) 'Properties -> Project Facets, uncheck Dynamic Web Module, then Ok or Apply. Then do Maven->Update Project.' para que tu __The Dynamic Web Module version__ se actualice a la versión 3.0 y te deje incluir el proyecto en los servidores.
