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
Descargar la versión apropiada desde la [página de descargas de Spark] (https://spark.apache.org/downloads.html). Yo me bajé spark-1.3.1-bin-hadoop2.6.tgz que traducido es spark 1.3.1 con hadoop 2.6 :). A día de hoy está la versión 1.4.0 de spark que viene con [Spark R] (https://spark.apache.org/docs/latest/api/R/index.html).

Una vez tengamos el fichero lo descomprimimos en un directorio a nuestra elección. Podemos probar que funciona correctamente lanzando ´bin/pyspark´.

##Enganchar spark con ipython notebooks
Básicamente he seguido este documento [base](https://districtdatalabs.silvrback.com/getting-started-with-spark-in-python). 
[enhaced](http://blog.cloudera.com/blog/2014/08/how-to-use-ipython-notebook-with-apache-spark/)
