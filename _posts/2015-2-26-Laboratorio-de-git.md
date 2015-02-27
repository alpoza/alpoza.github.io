---
layout: post
title: "Laboratorio de Git"
published: false
Category: dev
Tags: git
Authors: Alberto Poza
Summary: Modelo de ramas y worflow para empezar y no morir en el intento
---
**Modelo de ramas y worflow para empezar a usar git y no morir en el intento**

Si eres nuevo en git habrás oido hablar de sus bondades a la hora de trabajar con ramas, te habrán contado su facilidad para hacer merges automáticos, habrás visto que practicamente todos los proyectos open source están en [github](http://githab.com) y seguro que te habrán sugerido empezar a trabajar con git en lugar de cvs o svn.

Pero comenzar a utilizar git en un equipo de trabajo que nunca antes lo había utilizado, tiene una gran barrera de entrada:

- *aprender el funcionamiento y los comandos básicos*. Con cvs tenemos commit, update, branch, merge,... y todos estos comandos van contra el servidor central. En git tenemos muchas más comandos: clone, commit, push, pull, fetch, merge, rebase, reset, checkout,... algunos trabajan en local y otros en remoto, contra uno o varios repositorios.
- *workflow". El modo de trabajo con cvs es sencillo: te descargas lo que haya en el HEAD o BRANCH en el que tengas que trabajar y vas haciendo commits de los fuentes que se envian al respositorio central. Si hay conflictos en algún fichero cvs bloquea antes de subir. 
