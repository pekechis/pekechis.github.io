---
layout: post
title:  "GitHub Cli.#2 Configuración. Auto Completion"
lang: es
date: 2021-12-31 19:24:00 +0100
categories: cursos git github cli  autocompletion configuracion
---

Una vez hemos [instalado]() GitHub cli considero fundamental que conozcamos varias cosas:

* Dónde estás los archivos de configuración y qué van a contener dichas archivos de configuración.
* Cómo habilitar al auto-completion de GitHub Cli que me va a facilitar enormemente el flujo de trabajo a la hora de usar esta herramienta.
* Cómo obtener la ayuda para cada uno de los diferentes comando que nos proporciona GitHub Cli.

## Archivos de configuración.

Una vez hemos instalado GitHub Cli es importante conocer dónde están los archivos de configuración de la aplicación. Estos archivos se encuentra dentro del directorio **.config/gh/** (Windows?) que es un directorio oculto dentro de tu carpeta personal.

Dentro de este directorio nos encontraremos con los siguientes ficheros:

* **config.yml :** Donde se establecen el protocolo (HTTP/SSH), el paginador, el editor y el navegador preferidos, entre otras cosas.
* **hosts.yml :** Donde se establecen los servidores git y los datos . Normalmente será solo uno github.com.
* ¿¿¿¿¿¿Había alguno más?

## Auto-Completion.

Otro paso fundamental para trabajar con mayor soltura con la herramienta es habilitar el autocompletion de GitHub Cli. Usando el autocompletion obtendremos, pulsando el tabulador, las distintas opciones que tenemos para cada uno de los comandos y subcomandos.

Si hemos instalado GitHub Cli mediante el gestor de paquetes de mi sistema operativo seguramente no será necesario que hagamos ninguna operación adicional para habilitar el autocompletion. En caso contrario dependiento de nuestro shell deberemos ejecutar lo siguiente:

```sh

#Si nuestro shell en un BASH

#Sí nuestro shell es un ZSH

#Si nuestro shell es un fish

#Si nuestro shell es powershell

```

UNa vez lo tenemos funcionando 

