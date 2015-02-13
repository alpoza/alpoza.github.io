---
layout: post
title: "Sopa de Sonar"
published: flase
Category: dev
Tags: sonar
Authors: Alberto Poza
Summary: Si no te gusta la sopa... toma dos platos
---

mvn help:describe -DgroupId=org.codehaus.sonar -DartifactId=sonar-dev-maven-plugin -Ddetail=true -Dversion=1.7
mvn help:describe -DgroupId=org.codehaus.sonar -DartifactId=sonar-dev-maven-plugin -Ddetail=true -Dversion=1.8



mvn install org.codehaus.sonar:sonar-dev-maven-plugin:1.7:start-war -Dsonar.runtimeVersion=4.5.2 -Dlicense.skip=true

## Plugin Hot Deploy

Desde SonarQube 4.3 se permite un reinicio rápido del servidor cuando está configurado para arrancar en "development mode". Esto se indica incluyendo el parámetro `sonar.dev=true` en el fichero `conf/sonar.properties`. 

El siguiente comando se usa para desplegar una nueva versión del plugin en desarrollo. Es más rápido que reiniciar el servidor de la manera standard ya que el JRuby environment no se recarga. Ejecuta el siguiente comando desde el directorio donde reside el pom.xml del plugin:

    mvn package org.codehaus.sonar:sonar-dev-maven-plugin::upload -DsonarHome=/path/to/server/home -DsonarUrl=http://localhost:9000

Tambien puedes crear el jar de tu manera normal, copiarlo en el directorio de plugins de sonar y reiniciar sonar con una llamada a restart del Web API de Sonar:
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

## Arrancar maven en debug Mode

Otra de las cosas que te puede tocar hacer es depurar el código cuando se ejecuta el analizador batch
http://docs.sonarqube.org/display/SONAR/Debugging

Dependiendo si utilizas ant, Sonar Runner, Gradle o Maven los pasos son diferentes. La manera más cómoda es utilizar maven porque sólo requiere esta línea de comando:

    mvnDebug sonar:sonar 
    
Ahora sólo tienes que enlazar el depurador del IDE que utilices en el puerto que te muestre el comando anterior, habitualmente `localhost:8000`.
