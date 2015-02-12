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

## Plugin (Almost) Hot Deploy

SonarQube 4.3 allows to quickly restart server when the development mode is enabled (sonar.dev=true in conf/sonar.properties). It's used to deploy a new version of the plugin under development. It's a bit faster than restarting the server in a standard way (JRuby environment is not reloaded). Execute the following command from plugin sources :

    mvn package org.codehaus.sonar:sonar-dev-maven-plugin::upload -DsonarHome=/path/to/server/home -DsonarUrl=http://localhost:9000


## Arrancar maven en debug Mode

http://docs.sonarqube.org/display/SONAR/Debugging
'mvnDebug sonar:sonar and attach your IDE to the remote process'
