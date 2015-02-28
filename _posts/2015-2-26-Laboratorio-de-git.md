---
layout: post
title: "Laboratorio de Git"
published: false
Category: dev
Tags: git
Authors: Alberto Poza
Summary: Modelo de ramas y worflow para empezar y no morir en el intento
---
**Modelo de ramas y worflow para empezar a usar git y no morir en el intento**

## Soy nuevo en git
Si eres nuevo en git habrás oido hablar de sus bondades a la hora de trabajar con ramas, te habrán contado su facilidad para hacer merges automáticos, habrás visto que practicamente todos los proyectos open source están en [github](http://githab.com) y seguro que te habrán sugerido empezar a trabajar con git en lugar de cvs o svn.

Pero comenzar a utilizar git en un equipo de trabajo que nunca antes lo había utilizado, tiene una gran barrera de entrada:

- *aprender el funcionamiento y los comandos básicos*. Con cvs tenemos commit, update, branch, merge,... y todos estos comandos van contra el servidor central. En git tenemos muchas más comandos: clone, commit, push, pull, fetch, merge, rebase, reset, checkout,... algunos trabajan en local y otros en remoto, contra uno o varios repositorios.
- *workflow". El modo de trabajo con cvs es sencillo: actualizas o descargas en local lo que haya en el HEAD o BRANCH en el que tengas que trabajar y vas haciendo commits de los fuentes que se envian al respositorio central. Si hay conflictos en algún fichero al hacer un update o un commit, cvs bloquea antes de subir, resuelves el conflicto y lo vuelves a intentar. En git, aunque básicamente tienes que hacer lo mismo (actulizar tu copia local y subir tus cambios al repositorio/os), la forma de trabajo suele ser distinta: igualmente actualizas tu copia local con los cambios que haya en el repositorio, pero por norma general creas una nueva rama a partir de la principal. Los commits los haces en local y finalmente subes los cambios al respositorio, pero como estás en una rama tienes que hacer un merge con la principal. El modo en el que organices y pactes con el equipo de desarrollo esta "gestión" de ramas y el workflow a seguir, puede marcar la diferencia entre la excesiva sencillez y la extrema burocracia, por lo que hay que buscar el equilibrio para que todos nos sintamos confortables.

## El modelo ideal
El modelo ideal no existe, depende de las características del proyecto y de la experiencia del equipo de desarrollo.
Hay un excelente artículo describiendo [gitlab workflow](https://about.gitlab.com/2014/09/29/gitlab-flow/), con el que, aunque está orientado a trabajar con [gitlab](http://gitlab.org), puedes salir con las ideas claras.

La idea es tener la rama master como rama principal, o línea base y crear una rama por cada feature que desees añadir al desarrollo. La rama master es la que estará asociada a tu posible sitema de Integración Continua y estará [protegida](http://doc.gitlab.com/ee/permissions/permissions.html) para que no se pueda hacer un push directo a esta rama. Cada vez que termines una feature se deberá hacer un [merge request](https://gitlab.com/gitlab-org/gitlab-ce/merge_requests) para integrarla con la rama master y esta solicitud de merge request servirá como punto de unión, para hacer un review con el resto del equipo, como documentación de la resolución de cada feature, etc.
