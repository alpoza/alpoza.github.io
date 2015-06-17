---
layout: post
title: "ipython notebooks con spark"
published: true
Category: dev
Tags: python, ipython, notebooks, spark, edx
Authors: Alberto Poza
Summary: Configurar notebooks de ipython para utilizar spark en windows
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
Básicamente he seguido este documento [base](https://districtdatalabs.silvrback.com/getting-started-with-spark-in-python):

Crear un profile iPython notebook
  ipython profile create spark

Crear un fichero en el directorio creado para el profile 
$HOME/.ipython/profile_spark/startup/00-pyspark-setup.py con lo siguiente:

  import os
  import sys
  
  # Configure the environment
  if 'SPARK_HOME' not in os.environ:
      os.environ['SPARK_HOME'] = '/srv/spark'
  
  # Create a variable for our root path
  SPARK_HOME = os.environ['SPARK_HOME']
  
  # Add the PySpark/py4j to the Python Path
  sys.path.insert(0, os.path.join(SPARK_HOME, "python", "build"))
  sys.path.insert(0, os.path.join(SPARK_HOME, "python"))

Para que se cargue automáticamente el SparkContext al arrancar Ipython notebook me he basado en  [este documento](http://blog.cloudera.com/blog/2014/08/how-to-use-ipython-notebook-with-apache-spark/). Basicamente es añadir estas líneas al final del fichero anterior:
  sys.path.insert(0, os.path.join(SPARK_HOME, 'python/lib/py4j-0.8.1-src.zip'))
  execfile(os.path.join(SPARK_HOME, 'python/pyspark/shell.py'))

Ya podemoa arrancar Ipython notebook con el profile creado:

  ipython notebook --profile spark

Un problemilla que me ha surgido al realizar las prácticas es que matplotlib no funciona correctamente en el virtualenv. Parece ser que es la [incidencia está aun abierta] (https://github.com/pypa/virtualenv/issues/93), pero han encontrado un workarround que consiste en añadir dos enviroment variables en el fichero activate.bat del virtualenv que estemos utilizando, ajustando correctamente los directorios a nuestra instalación de python, en mi caso:
  set TCL_LIBRARY=C:\apps\Anaconda\tcl\tcl8.5
  set TK_LIBRARY=C:\apps\Anaconda\tcl\tk8.5
