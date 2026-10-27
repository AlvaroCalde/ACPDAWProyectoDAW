- [CLIENTE DE DESARROLLO - WINDOWS 10/11](#cliente-de-desarrollo---windows-1011)
  - [**1 Configuración inicial**](#1-configuración-inicial)
  - [**2 Navegadores**](#2-navegadores)
  - [**3 MobaXterm**](#3-mobaxterm)
    - [*Instalación*](#instalación)
    - [*Configuración*](#configuración)
    - [*Ejemplo de uso*](#ejemplo-de-uso)
      - [**Conexion SSH**](#conexion-ssh)
      - [**Conexión SFTP**](#conexión-sftp)
  - [**4 Netbeans**](#4-netbeans)
    - [*Instalación*](#instalación-1)
    - [*Configuración*](#configuración-1)
    - [*Ejemplo de uso*](#ejemplo-de-uso-1)
  - [**5 Visual Studio Code**](#5-visual-studio-code)
    - [*Instalación*](#instalación-2)
    - [*Configuración*](#configuración-2)


## <h2>**1 Configuración inicial**</h2>

Como estamos en el dominio de la Junta hay poco que explicar, tenemos Windows 10 con 16 GB de RAM.

## <h2>**2 Navegadores**</h2>
De nuevo, como estamos en el dominio de la Junta esto será breve.       
La cuenta administradora es inaccesible para nosotros así que solo tenemos una cuenta de desarrollador, la nuestra propia, en mi caso es alvaro.calper.    

## <h2>**3 MobaXterm**</h2>

### <h2>*Instalación*</h2>
https://mobaxterm.mobatek.net/download-home-edition.html

Lo descargamos de la página oficial, tenemos dos versiones, una portable y otra para instalar, yo uso la portable.

### <h2>*Configuración*</h2>

### <h2>*Ejemplo de uso*</h2>

#### **<h3>Conexion SSH</h3>**
Damos al icono de *Session* arriba a la izquierda, elegimos SSH y configuramos ip y nombre de usuario.     
<img src="../webroot/images/moba01.png" width="600px">
Damos al icono de *Session* arriba a la izquierda, elegimos SSH y configuramos ip y nombre de usuario.

Le damos a OK y nos pedirá la contraseña para conectarse.

Si es la primera vez que configuramos una sesión no ssaldrá otra ventana para guardar las contraseñas, si aceptamos nos pedira que pongamos una contraseña general para el programa, si no aceptamos tendremos que poner la contraseña cada vez que nos conectemos.

Nos conectamos seleccionando en la lista de *User sessions* y se abrirá una consola lista con nuestro usuario para poner comandos
````Bash
miadmin@acp-used:~$
````

#### **<h3>Conexión SFTP</h3>**
Damos al icono de *Session* arriba a la izquierda, elegimos SFTP y configuramos ip y nombre de usuario.    
<img src="../webroot/images/moba02.png" width="600px">
Damos al icono de *Session* arriba a la izquierda, elegimos SFTP y configuramos ip y nombre de usuario.

Le damos a OK y nos pedirá la contraseña para conectarse.

Una vez configurado saldra en la lista de *User sessions* para poder usarlas.

Lo seleccionamos en la lista y se abrirá una ventana con la parte local a la izquierda para buscar los archivos que queremos subir y la parte del servidor a la derecha donde tenemos iconos para ir **arriba**, **actualizar**, **crear carpetas**, **crear archivos**, **abrir carpeta**, **borrar elementos**, **subir archivo** (se abre el explorador de windows para elegir un archivo), **descargar archivo** (seleccionamos un archivo, le damos al boton y se abre el explorador de windows para elegir donde guardarlo) y la barra que indica la ruta donde estamos.

También podemos abrir cualquier archivo de texto, html, php, etc (con doble click) con el editor de texto de MobaXterm por si tenemos que hacer alguna modificación menor o ver el código.

## <h2>**4 Netbeans**</h2>

### <h2>*Instalación*</h2>
Apache NetBeans IDE 20

### <h2>*Configuración*</h2>
- configurar carpetas por defecto, cambiar las rutas

### <h2>*Ejemplo de uso*</h2>
- version y plugins de netbeans (las que esten por defecto, donde se ve)
- como usar github
- como llevar a casa y volver a usar aquí (es github)
- capturas solo de la parte ineteresada, no de toda la pantalla

**<h3>Crear proyecto con conexion (SFTP) al servidor</h3>**
- Nuevo proyecto PHP marcando la opción "PHP Application from Remote Server"    
<img src="../webroot/images/NB01.png" width="600px">

- Ponemos nombre de proyecto y cambiamos la ruta por la nuestra personal   
<img src="../webroot/images/NB02.png" width="600px">

- Configuramos la url con nuestro servidor y el nombre del proyecto en el servidor, en el caso del proyecto principal no tiene carpeta es la raíz (/)     
<img src="../webroot/images/NB03.png" width="600px">

- La primera vez que creamos un proyecto con conexión al servidor hay que configurarlo con nombre de usuario, contraseña y directorio inicial     
<img src="../webroot/images/NB04.png" width="600px">
- Nuevo proyecto PHP marcando la opción "PHP Application from Remote Server"

- Ponemos nombre de proyecto y cambiamos la ruta por la nuestra personal

- Configuramos la url con nuestro servidor y el nombre del proyecto en el servidor, en el caso del proyecto principal no tiene carpeta es la raíz (/)

- La primera vez que creamos un proyecto con conexión al servidor hay que configurarlo con nombre de usuario, contraseña y directorio inicial

- Si todo ha ido bien se conectará al servidor y entrará en la carpeta del proyecto que previamente tenemos que haber creado y que haya por lo menos un archivo en ella. Aquí ya marcamos para que se bajen los archivos que queramos.

**<h3>Borrar proyecto con conexion (SFTP) al servidor</h3>**
- botón secundario sobre el proyecto y delete. Nos pedirá confirmación y si queremos que borre los archivos en nuestro ordenador. Los del servidor lo tenemos que borrar a mano.


## <h2>**5 Visual Studio Code**</h2>

### <h2>*Instalación*</h2>
https://code.visualstudio.com/

En la pagina oficial le damos a *Download for Windows* para descargar la última version disponible. 
Abrimos el ejecutable y todo siguiente, siguiente y terminar.

### <h2>*Configuración*</h2>

**<h3>Cambiar idioma a español</h3>**
https://marketplace.visualstudio.com/items?itemName=MS-CEINTL.vscode-language-pack-es

Instalamos esa extensión y le damos a "change languaje and restart" en la ventana emergente que sale abajop a la derecha.

**<h3>Auto Close Tag (Extensión)</h3>**
https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag

**<h3>Git Graph (Extensión)</h3>**
https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph

**<h3>Live Server (Extensión)</h3>**
https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer

**<h3>Markdown All in One (Extensión)</h3>**
https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one

**<h3>PHP Extension Pack (Extensión)</h3>**
https://marketplace.visualstudio.com/items?itemName=xdebug.php-pack&ssr=false#overview

Esta extensión instala otras dos, la PHP IntelliSense

https://marketplace.visualstudio.com/items?itemName=zobo.php-intellisense

y PHP Debug

https://marketplace.visualstudio.com/items?itemName=xdebug.php-debug

**<h3>PHP Intelephense (Extensión)</h3>**
https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client

Para que funcione hay deshabilitar la que viene por defecto, buscamos @builtin php en el buscador de extensiones y la desactivamos.

**<h3>SFTP (Extensión)</h3>**
https://marketplace.visualstudio.com/items?itemName=Natizyskunk.sftp

**<h3>vscode-pdf (Extensión)</h3>**
https://marketplace.visualstudio.com/items?itemName=tomoki1207.pdf



