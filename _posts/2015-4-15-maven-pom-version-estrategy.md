---
layout: post
title: "Estrategia para cambiar el id de versión para proyectos maven"
published: true
Category: dev
Tags: maven, jenkins, versions
Authors: Alberto Poza
Summary: distintas formas de versionar proyectos maven
---
*Estrategia para cambiar el id de versión para proyectos maven**

##Utilizando jenkins
con la ayuda de build-helper maven plugin y versions podemos añadir el build_number a la versión del proyecto:
`mvn build-helper:parse-version versions:set -DnewVersion=${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}-${BUILD_NUMBER}`

  
  


