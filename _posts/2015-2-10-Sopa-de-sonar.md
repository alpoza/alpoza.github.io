---
layout: post
title: "Sopa de Sonar"
published: true
Category: dev
Tags: sonar
Authors: Alberto Poza
Summary: Si no te gusta la sopa... toma dos platos
---
**Si no te gusta la sopa... toma dos platos**

Ahora mismo estoy involucrado en varios frentes relacionados con SonarQube:
- Migración de la plataforma Sonarqube 4.1 a 4.5.2 LTS
- Compatibilidad de los plugins implementados en 4.1
- Desarrollo de nuevas reglas

Durante el desarrollo de estas tareas han surgido algunas dificultades que con la ayuda de google hemos ido solucionando y que dejo aquí anotadas por si fueran de utilidad futura.

## Plugin Hot Deploy

Algo que te va a suceder, si estás probando un plugin para SonarQube, es *desplegar el plugin en el servidor* para poder probarlo. Esto implica, generar el jar, copiar el jar en el directorio de extensiones de SonarQube y parar y arrancar el servidor. Así que busqué formas de automatizar y acelerar este proceso ya que lo normal es tener que hacerlo en numerosas ocasiones durante el ciclo de vida del desarrollo del plugin.

Desde SonarQube 4.3 se permite un reinicio rápido del servidor cuando está configurado para arrancar en "development mode". Esto se indica incluyendo el parámetro `sonar.dev=true` en el fichero `conf/sonar.properties`. 

El siguiente comando se usa para desplegar una nueva versión del plugin en desarrollo. Es más rápido que reiniciar el servidor de la manera standard ya que el JRuby environment no se recarga. Ejecuta el siguiente comando desde el directorio donde reside el pom.xml del plugin:

    mvn package org.codehaus.sonar:sonar-dev-maven-plugin::upload -DsonarHome=/path/to/server/home -DsonarUrl=http://localhost:9000

También puedes crear el jar de tu manera normal, copiarlo en el directorio de plugins de sonar (con algún plugin de despliegue de maven) y reiniciar sonar con una llamada a restart del Web API de Sonar:
`curl -v --noproxy localhost -XPOST http://localhost:9000/api/system/restart`

@TODO: a mi me fallan ambos métodos. Utilizando curl tendremos un error más descriptivo:

    org.jruby.exceptions.RaiseException: (NoMethodError) undefined method `controllers' for nil:NilClass
            at org.jruby.RubyKernel.method_missing(org/jruby/RubyKernel.java:255)
            at RUBY.method_missing(C:/apps/sonarqube-4.5.2/web/WEB-INF/gems/gems/activesupport-2.3.15/lib/active_support/whiny_nil.rb:52)
            at RUBY.add_java_ws_routes(C:/apps/sonarqube-4.5.2/web/WEB-INF/config/../lib/java_ws_routing.rb:34)
            at RUBY.reload(C:/apps/sonarqube-4.5.2/web/WEB-INF/config/../lib/java_ws_routing.rb:55)
            at RUBY.reload_application(C:/apps/sonarqube-4.5.2/web/WEB-INF/gems/gems/actionpack-2.3.15/lib/action_controller/dispatcher.rb:58)
            at RUBY.run(C:/apps/sonarqube-4.5.2/web/WEB-INF/gems/gems/actionpack-2.3.15/lib/action_controller/reloader.rb:42)
            at RUBY.call(C:/apps/sonarqube-4.5.2/web/WEB-INF/gems/gems/actionpack-2.3.15/lib/action_controller/dispatcher.rb:108)
            at RUBY.serve_rails(file:/C:/apps/sonarqube-4.5.2/lib/server/jruby-rack-1.1.13.2.jar!/rack/adapter/rails.rb:34)
            at RUBY.call(file:/C:/apps/sonarqube-4.5.2/lib/server/jruby-rack-1.1.13.2.jar!/rack/adapter/rails.rb:39)
            at RUBY.call(file:/C:/apps/sonarqube-4.5.2/lib/server/jruby-rack-1.1.13.2.jar!/rack/handler/servlet.rb:22)

Por otro lado utilizando la versión 1.7 de sonar-dev-maven-plugin, teóricamente era posible instalar el plugin y lanzar la web en un único comando:

    mvn install org.codehaus.sonar:sonar-dev-maven-plugin:1.7:start-war -Dsonar.runtimeVersion=4.5.2
    
Lamentablemente intenta descargar `[get] Error getting http://dist.sonar.codehaus.org/sonar-4.5.2.zip` mientras que en [el servidor de distribuciones](http://dist.sonar.codehaus.org/) el fichero para la versión 4.5.2 se llama `sonarqube-4.5.2.zip` 

Si queréis ver la ayuda de lo que se puede hacer con sonar-dev-maven-plugin:

    mvn help:describe -DgroupId=org.codehaus.sonar -DartifactId=sonar-dev-maven-plugin -Ddetail=true -Dversion=1.7
    mvn help:describe -DgroupId=org.codehaus.sonar -DartifactId=sonar-dev-maven-plugin -Ddetail=true -Dversion=1.8

## Arrancar maven en debug Mode

Otra de las cosas que te puede tocar hacer es depurar el código cuando se ejecuta el analizador batch. La información oficial para depurar se puede encontrar en [Sonar Debugging](http://docs.sonarqube.org/display/SONAR/Debugging)

Dependiendo si utilizas ant, Sonar Runner, Gradle o Maven los pasos son diferentes. La manera más cómoda es utilizar maven porque sólo requiere esta línea de comando:

    mvnDebug sonar:sonar 
    
Ahora sólo tienes que enlazar el depurador del IDE que utilices en el puerto que te muestre el comando anterior, habitualmente `localhost:8000`.

## Resolver conflictos con dependencias
Para implementar un plugin en el que añadir nuevas reglas de validación en Java (u otros lenguajes), tienes que utilizar unas cuantas librerías que proporciona SonarQube, de las que puede haber varias versiones de cada una de ellas y pueden existir incompatibilidades entre ellas y/o con los propias librerias core de la distribución. Si además estás migrando un plugin existente para una versión anterior de SonarQube, que ya utilizan sus propias versiones de las librerías, el caos puede llegar cuando menos te lo esperes si no tienes cuidado.

Si estás utilizando maven, la manera más cómoda para ver las versiones de cada librería que están utilizando en tu proyecto es utilizar el plugin de maven para eclipse, abrir el `pom.xml` correspondiente y ver la lengueta dependency hierarchy. Si no se tiene a mano eclipse siempre podemos utilizar maven directamente para ver el [árbol de dependencias](http://maven.apache.org/plugins/maven-dependency-plugin/examples/resolving-conflicts-using-the-dependency-tree.html):

    mvn dependency:tree -Dverbose -Dincludes=org.codehaus.sonar.sslr-squid-bridge:sslr-squid-bridge

     --- maven-dependency-plugin:2.8:tree (default-cli) @ java-custom-rules ---
     org.sonar.samples:java-custom-rules:sonar-plugin:1.0-SNAPSHOT
     \- org.codehaus.sonar-plugins.java:sonar-java-plugin:sonar-plugin:2.9.1:provided
        \- org.codehaus.sonar-plugins.java:java-squid:jar:2.9.1:provided
           \- org.codehaus.sonar.sslr-squid-bridge:sslr-squid-bridge:jar:2.4:provided

## Exclusions
Mulitcriteria exclusions vía Maven:

    <properties>
            <sonar.issue.ignore.multicriteria>e1,e2</sonar.issue.ignore.multicriteria>
            <sonar.issue.ignore.multicriteria.e1.ruleKey>squid:key</sonar.issue.ignore.multicriteria.e1.ruleKey>
            <sonar.issue.ignore.multicriteria.e1.resourceKey>**/*Nombre.java</sonar.issue.ignore.multicriteria.e1.resourceKey>
            <sonar.issue.ignore.multicriteria.e2.ruleKey>squid:otraKey</sonar.issue.ignore.multicriteria.e2.ruleKey>
            <sonar.issue.ignore.multicriteria.e2.resourceKey>**/OtroNombre.java</sonar.issue.ignore.multicriteria.e2.resourceKey>
    </properties>

o en Sonar Runner:

    sonar.issue.ignore.block=e1,e2
    sonar.issue.ignore.block.e1.beginBlockRegexp=@SonarIgnorexpINI
    sonar.issue.ignore.block.e1.endBlockRegexp=@SonarIgnorexpEND
    sonar.issue.ignore.block.e2.beginBlockRegexp=@GeneralIgnorexpINI
    sonar.issue.ignore.block.e2.endBlockRegexp=@GeneralIgnorexpEND

Si queremos excluir ficheros autogenerados:

    <sonar.exclusions>
        file:**/src-gen/**,
        file:**/emf-gen/**,
        file:**/example/**,
        file:**/tests/**,
        **/*RuntimeModule.java,
        **/*UiModule.java,
        **/*XcoreReader.java,
        **/*UiExamples.java,
        **/*TypeSystemGen*.java,
        **/*StandaloneSetup*.java
    </sonar.exclusions>
