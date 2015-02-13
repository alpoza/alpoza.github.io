---
layout: post
title: "Sopa de Sonar"
published: flase
Category: dev
Tags: sonar
Authors: Alberto Poza
Summary: Pesadilla de Sonar
---

mvn help:describe -DgroupId=org.codehaus.sonar -DartifactId=sonar-dev-maven-plugin -Ddetail=true -Dversion=1.7
mvn help:describe -DgroupId=org.codehaus.sonar -DartifactId=sonar-dev-maven-plugin -Ddetail=true -Dversion=1.8



mvn install org.codehaus.sonar:sonar-dev-maven-plugin:1.7:start-war -Dsonar.runtimeVersion=4.5.2 -Dlicense.skip=true

## Plugin Hot Deploy

Desde SonarQube 4.3 se permite un reinicio rápido del servidor cuando está configurado para arrancar en "development mode". Esto se indica incluyendo el parámetro `sonar.dev=true` en el fichero `conf/sonar.properties`. 

El siguiente comando se usa para desplegar una nueva versión del plugin en desarrollo. Es más rápido que reiniciar el servidor de la manera standard ya que el JRuby environment no se recarga. Ejecuta el siguiente comando desde el directorio donde reside el pom.xml del plugin:

    mvn package org.codehaus.sonar:sonar-dev-maven-plugin::upload -DsonarHome=/path/to/server/home -DsonarUrl=http://localhost:9000


## Arrancar maven en debug Mode

Otra de las cosas que te puede tocar hacer es depurar el código cuando se ejecuta el analizador batch
http://docs.sonarqube.org/display/SONAR/Debugging

Dependiendo si utilizas ant, Sonar Runner, Gradle o Maven los pasos son diferentes. La manera más cómoda es utilizar maven porque sólo requiere esta línea de comando:

    mvnDebug sonar:sonar 
    
Ahora sólo tienes que enlazar el depurador del IDE que utilices en el puerto que te muestre el comando anterior, habitualmente `localhost:8000`.
