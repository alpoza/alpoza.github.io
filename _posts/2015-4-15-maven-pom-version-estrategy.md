---
layout: post
title: "Estrategia para cambiar el id de versión para proyectos maven"
published: true
Category: dev
Tags: maven, jenkins, versions
Authors: Alberto Poza
Summary: distintas formas de versionar proyectos maven
---

##Utilizando jenkins
con la ayuda de build-helper maven plugin y versions podemos añadir el build_number a la versión del proyecto:
`mvn build-helper:parse-version versions:set -DnewVersion=${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}-${BUILD_NUMBER}`

El problema es que el comando anterior modifica la versión en el pom.xml. Como tenemos un proyecto multimodule tendríamos que modificar la versión del padre en cada uno de los módulos hijos. Además sería preferible que la solución no modificase ninguno de los artefactos del proyecto al lanzar la build.

Una posible solución sería utilizar una varible en la sección properties del pom.xml parent que se llame por ejemplo project.version:

	<version>0.0-SNAPSHOT</version>
	<properties>
		<project.deploy.version>${project.version}</project.deploy.version>
	</properties>

En cada módulo asignamos la versión utilizamos esta propiedad:

	<version>${project.deploy.version}</version>

De esta forma la versión del parent es 0.0-SNAPSHOT así como la de todos los módulos por defecto, pero si al hacer la build asignamos un valor a la variable mediante `-Dproject.deploy.version=0.0-${BUILD_NUMBER}` tendremos una versión diferene para cada uno de los módulos. Podemos ayudarnos de build-helper maven plugin para la major minor version la obtenga del pom.xml parent: 
`mvn build-helper:parse-version clean package -Dproject.deploy.version=${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}-${BUILD_NUMBER}`


