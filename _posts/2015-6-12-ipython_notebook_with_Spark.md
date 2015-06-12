---
layout: post
title: "ipython notebooks con spark"
published: true
Category: dev
Tags: python, ipython, notebooks, spark, edx
Authors: Alberto Poza
Summary: Configurar notebooks de ipython para ejecutar spark
---
**big big data**
Estoy haciendo el curso `[BerkeleyX: CS100.1x Introduction to Big Data with Apache Spark](https://courses.edx.org/courses/BerkeleyX/CS100.1x/1T2015/courseware)

Bastante interesante si no conoces Spark y muy introductorio si te gusta todo lo relacionado con Big Data. El problema es que para las prácticas de laboratorio utilizan una imagen que se carga mediante Vagrant.
Otro día contaré mi idilio con Vagrant, pero lo principal es que mi eeepc no puede con esa imagen por lo que he tenido que buscarme un workarround o lo que es lo mismo, buscarme la vida.

Como los ejercicios se realizan en un ipython notebook enganchado con Spark, aquí dejo los pasos que a mi me han funcionado para configurar el entorno:

##Instalar ipython notebook
Yo utilizo python 2.7 mediante la distribución Anaconda. Anaconda ya viene con un ipython out of the box, así que vamos a utilizarlo.
Primer problema: a la hora de importar un notebook salta un error 500... WTF
Como no me apetece ivestigar, creo que lo más rápido es reinstalarme ipython con las últimas versiones.
Como ya tengo virtualenv y virtualenvwrapper, imprescindible para aislar las dependencias de cada proyecto python, nos ponemos manos a la obra:

  mkvirtualenv spark
  workon spark
  pip install ipython[All]
  ipython notebook
  
http://localhost:8888

importamos un notebook y... Funciona !!! :)

##Instalar spark

##Enganchar spark con ipython notebooks
[base](https://districtdatalabs.silvrback.com/getting-started-with-spark-in-python)
[enhaced](http://blog.cloudera.com/blog/2014/08/how-to-use-ipython-notebook-with-apache-spark/)
