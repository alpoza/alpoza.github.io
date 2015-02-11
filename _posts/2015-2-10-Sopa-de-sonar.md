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
