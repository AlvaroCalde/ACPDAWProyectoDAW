## Índice

- [1. Introducción](#1-introducción)
- [2. Primeros Pasos](#2-primeros-pasos)
  - [2.1 Acceso a Plesk](#21-acceso-a-plesk)
  - [2.2 Panel de control](#22-panel-de-control)
- [3. Gestión de Dominios](#3-gestión-de-dominios)
  - [3.1 Crear subdominios](#31-crear-subdominios)
  - [3.2 Configuración DNS](#32-configuración-dns)
- [4. Despliegue de Aplicaciones](#4-despliegue-de-aplicaciones)
- [5. Gestión de Bases de Datos](#5-gestión-de-bases-de-datos)
  - [5.1 Crear base de datos en Plesk](#51-crear-base-de-datos-en-plesk)
  - [5.2 Importar datos (volcado SQL)](#52-importar-datos-volcado-sql)
  - [5.3 Gestión con phpMyAdmin](#53-gestión-con-phpmyadmin)
  - [5.4 Configuración de conexión](#54-configuración-de-conexión)
- [6. Acceso Avanzado](#6-acceso-avanzado)
  - [6.1 Conexión SFTP](#61-conexión-sftp)
  - [6.2 Conexión SSH](#62-conexión-ssh)
- [7. Configuración Avanzada](#7-configuración-avanzada)
  - [7.1 Archivo .htaccess](#71-archivo-htaccess)

---

## 1. Introducción

**¿Qué es Plesk?**

Plesk es un panel de control web para administrar servidores y sitios web fácilmente. Permite gestionar dominios, correos, bases de datos y archivos desde una interfaz gráfica. Incluye opciones para instalar aplicaciones, configurar seguridad y automatizar tareas del servidor.

---

## 2. Primeros Pasos

### 2.1 Acceso a Plesk
**Pasos:**

1. Accede al enlace: https://plesk.ieslossauces.es/login_up.php?success_redirect_url=%2Fsmb%2Fweb%2Fview
2. Introduce tu **usuario** y **contraseña**
3. Haz clic en **Iniciar sesión**

Una vez dentro, llegarás a la página principal de gestión del hosting.

---

### 2.2 Panel de control

El panel de control de Plesk está organizado en secciones accesibles desde el menú lateral izquierdo:

- **Sitios web y dominios**: Gestión de dominios, subdominios y configuración de sitios
- **Archivos**: Gestor de archivos para subir contenido
- **Bases de datos**: Creación y gestión de bases de datos
- **FTP**: Configuración de acceso FTP/SFTP
- **Git**: Integración con repositorios Git
- **SSH Terminal**: Acceso a la consola del servidor

---

## 3. Gestión de Dominios

### 3.1 Crear subdominios

Los subdominios permiten organizar diferentes proyectos o secciones de tu sitio web bajo el mismo dominio principal.

**Pasos:**

1. En el panel de Plesk, ve a **Sitios web y dominios**
2. Haz clic en **Añadir subdominio**
3. Completa la configuración:
   - **Nombre del subdominio**: Escribe el nombre del subdominio (normalmente el del proyecto que quieres subir)
   - **Raíz del documento**: Elige el directorio para guardar el proyecto

**Importante sobre la ruta del documento:**

En nuestro caso será dentro de `httpdocs`. Por ejemplo, si queremos poner la aplicación al tema 4 de DWES, la ruta sería:
```
/httpdocs/JTGDWESProyectoTema4
```

**Nota crítica:** Asegúrate de que el nombre de la carpeta sea solo el del proyecto, ya que por defecto Plesk pone el dominio entero a esta carpeta. Tendrás que borrarlo antes de darle aceptar.

4. Haz clic en **Aceptar**

Una vez creado, el subdominio estará disponible y apuntará a la carpeta especificada.

---

### 3.2 Configuración DNS

Para crear registros DNS personalizados (por ejemplo, para sitios virtuales):

**Pasos:**

1. En **Sitios web y dominios**, selecciona tu dominio principal
2. Ve al apartado **Hosting y DNS**
3. Haz clic en **DNS**
4. Haz clic en **Añadir registro**
5. Configura el registro:
   - **Tipo de registro**: Selecciona **A**
   - **Nombre de dominio**: Escribe el nombre que quieras para el subdominio
   - **TTL**: Dejar por defecto
   - **Dirección IP**: Introduce la dirección IP de tu servidor
6. Haz clic en **Aceptar**
7. Plesk te indicará que hay que actualizar los registros DNS. Haz clic en **Actualizar**

El registro DNS ya estará activo. Este método es útil cuando necesitas configurar sitios virtuales en el servidor de desarrollo.

---

## 4. Despliegue de Aplicaciones

### 4.1 Método 1: Subida manual (FTP/SFTP)

Este método es ideal para subidas rápidas o cuando no trabajas con control de versiones.

**Configurar acceso FTP:**

1. En el panel de Plesk, ve a **Sitios web y dominios**
2. Haz clic en **FTP**
3. Selecciona tu cuenta de usuario (haz clic en tu nombre)
4. Configura los datos de conexión:
   - **Nombre de usuario**: Visualiza tu nombre de usuario
   - **Contraseña**: Establece o cambia la contraseña
   - **Acceso SSH**: Verifica que sea `/bin/bash`
   - **Dirección IP**: Anota la IP para la conexión
5. Haz clic en **Guardar**

**Conexión con cliente FTP/SFTP:**

Puedes usar cualquiera de estos clientes para conectarte:
- **FileZilla**
- **WinSCP**
- **MobaXterm**

**Datos de conexión:**
- **Protocolo**: SFTP (recomendado) o FTP
- **Host**: Dirección IP anotada anteriormente
- **Usuario**: Tu nombre de usuario
- **Contraseña**: La contraseña configurada
- **Puerto**: 22 (SFTP) o 21 (FTP)

**Subir archivos:**

1. Conecta con el cliente FTP/SFTP usando los datos anteriores
2. Navega hasta la carpeta destino en el servidor (por ejemplo: `/httpdocs/nombre-proyecto`)
3. Selecciona los archivos y carpetas de tu proyecto local
4. Arrástralos o súbelos según el cliente que uses

**Alternativa desde Plesk:**

También puedes subir archivos directamente desde la interfaz de Plesk:

1. Ve al menú lateral izquierdo y haz clic en **Archivos**
2. Navega hasta la carpeta `httpdocs` del directorio principal
3. Entra en la carpeta `httpdocs`
4. Arrastra las carpetas desde tu carpeta del proyecto local directamente a la interfaz web


---

## 5. Gestión de Bases de Datos

### 5.1 Crear base de datos en Plesk

**Pasos:**

1. En el panel de Plesk, ve a **Sitios web y dominios**
2. Haz clic en **Bases de datos**


3. Haz clic en **Añadir base de datos**


4. Completa el formulario:

   - **Nombre de la base de datos**: Introduce el nombre de la base de datos
   - **Sitio relacionado**: Selecciona el subdominio asociado al proyecto (opcional pero recomendado)
   - **Usuario de la base de datos**: Crea un nuevo usuario
     - **Nombre de usuario**: Introduce el nombre del usuario
     - **Contraseña**: Crea una contraseña segura que cumpla con los requisitos de Plesk
     - **Confirmar contraseña**: Repite la contraseña


5. Haz clic en **Aceptar** o **Crear**

Una vez creada, la base de datos aparecerá en la lista y tendrás la opción de abrirla en **phpMyAdmin**.

---

### 5.2 Importar datos (volcado SQL)

Una vez creada la base de datos, puedes importar la estructura y los datos iniciales.

**Preparar archivos:**

1. Crea un archivo `.sql` con los scripts de:
   - Creación de tablas
   - Carga inicial de datos (si la hay)
   - (Opcionalmente, script de borrado)

2. Comprime el archivo `.sql` en formato **ZIP**

**Importar en Plesk:**

1. En la lista de bases de datos, selecciona la base de datos que acabas de crear
2. Haz clic en **Importar volcado**


3. Configura la importación:
   - **Ubicación**: Selecciona **Desde el ordenador local**
   - Haz clic en **Seleccionar un archivo .sql .zip**
   - Selecciona el archivo ZIP que preparaste


4. Haz clic en **Importar volcado**

Plesk procesará el archivo y ejecutará los scripts SQL en la base de datos.

**Verificar importación:**

1. En la lista de bases de datos, haz clic en **phpMyAdmin**


2. En phpMyAdmin, verás la base de datos en el panel lateral izquierdo
3. Haz clic en la base de datos y verifica que las tablas se hayan creado correctamente

---

### 5.3 Gestión con phpMyAdmin

phpMyAdmin es una herramienta web para gestionar bases de datos MySQL/MariaDB.

**Acceso:**

1. Desde Plesk, ve a **Bases de datos**
2. Selecciona la base de datos que quieres gestionar
3. Haz clic en **phpMyAdmin**

Se abrirá phpMyAdmin ya autenticado en la base de datos seleccionada. Solo podrás gestionar esa base de datos específica, no verás todas las bases de datos del servidor.

**Ejecutar consultas SQL:**

1. Ve a la pestaña **SQL** en la parte superior
2. Escribe o copia el código SQL que quieras ejecutar en el área de texto
3. **Guardar consulta en favoritos (recomendado):**
   - Antes de ejecutar, en la parte inferior de la página marca **Guardar esta consulta en favoritos**
   - Ponle un nombre descriptivo (por ejemplo: "Crear tablas", "Carga inicial", "Borrar datos")
4. Haz clic en **Continuar** en la parte inferior derecha para ejecutar la consulta

**Reutilizar consultas guardadas:**

Una vez guardadas, aparece una nueva sección en la parte inferior: **Consulta guardada en favoritos**

- Despliega la lista y selecciona la consulta que quieras
- Haz clic en **Continuar** para ejecutarla de nuevo

Esto es muy útil para scripts que ejecutas frecuentemente, como resetear la base de datos o cargar datos de prueba.

---

### 5.4 Configuración de conexión

**Datos de conexión:**

Para conectar tu aplicación PHP con la base de datos creada en Plesk, utiliza los siguientes datos:

- **Host**: `localhost`
- **Nombre de la base de datos**: El nombre que indicaste al crear la base de datos
- **Usuario**: El usuario que creaste al configurar la base de datos
- **Contraseña**: La contraseña que estableciste para ese usuario

**Separación de entornos:**

El objetivo es que el archivo de configuración en el entorno de **explotación** (rama `master`) no tenga las contraseñas, usuarios ni datos de la base de datos directamente en el código, a diferencia del entorno de **desarrollo** (rama `developer` o `develop`) que puede tenerlos para facilitar el trabajo local.

Como el resto del código PHP es el mismo en los dos entornos, ambos usan el mismo archivo de configuración (por ejemplo: `confDBPDO.php`).


## 6. Acceso Avanzado

### 6.1 Conexión SFTP

SFTP (SSH File Transfer Protocol) permite transferir archivos de forma segura usando el protocolo SSH.

**Obtener datos de conexión:**

Si no tienes los datos de conexión, sigue estos pasos:

1. En Plesk, ve a **Sitios web y dominios**
2. Haz clic en **FTP**
3. Selecciona tu cuenta (haz clic en tu nombre de usuario)
4. En la ventana de configuración verás:
   - **Nombre de usuario**
   - **Dirección IP del servidor**
   - **Acceso SSH**: Debería ser `/bin/bash`

5. Configura o cambia la contraseña si es necesario
6. Haz clic en **Guardar**

**Conectar con MobaXterm (u otro cliente SFTP):**

1. Abre MobaXterm
2. Haz clic en **Session** → **SFTP**
3. Introduce los datos:
   - **Remote host**: Dirección IP del servidor
   - **Username**: Tu nombre de usuario
   - **Port**: 22
4. Haz clic en **OK**
5. Introduce la contraseña cuando te la pida

Ya estarás conectado y podrás transferir archivos de forma segura.

---

### 6.2 Conexión SSH

SSH permite acceder a la línea de comandos del servidor para ejecutar comandos directamente.

**Opción 1: Desde la terminal de Plesk**

Esta es la forma más rápida y no requiere configuración adicional.

1. En Plesk, ve a **Sitios web y dominios**
2. Haz clic en **SSH Terminal**

<!-- Captura sugerida: botón SSH Terminal en el menú de Plesk -->

3. Se abrirá la consola de comandos directamente en el navegador
4. Ya puedes ejecutar comandos según los permisos que tengas

<!-- Captura sugerida: terminal SSH abierta en el navegador mostrando el prompt -->

**Opción 2: Desde un cliente SSH externo (MobaXterm u otro)**

1. Abre MobaXterm
2. Haz clic en **Session** → **SSH**
3. Introduce los datos:
   - **Remote host**: Dirección IP del servidor
   - **Username**: Tu nombre de usuario (el mismo de SFTP)
   - **Port**: 22
4. Haz clic en **OK**
5. Introduce la contraseña cuando te la pida

Ya estarás conectado a la terminal del servidor y podrás ejecutar comandos.

---

## 7. Configuración Avanzada

### 7.1 Archivo .htaccess

El archivo `.htaccess` es un archivo de configuración de Apache que permite modificar el comportamiento del servidor web para un directorio específico.

**Ubicación:** Raíz del proyecto (dentro de `httpdocs/nombre-proyecto`)

**Usos comunes:**

#### **1. Redirección a índice personalizado**

Especifica qué archivo debe cargarse por defecto cuando se accede al directorio:
```apache
directoryindex indexProyectoCIB.php
```

#### **2. Habilitar autenticación HTTP en PHP**

Para que funcione el método `header('WWW-Authenticate: Basic realm="Contenido restringido"')` en PHP:
```apache
<IfModule mod_rewrite.c>
    RewriteEngine on
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
</IfModule>
```

#### **3. Páginas de error personalizadas**

Redirige los errores HTTP a páginas personalizadas en la carpeta `error` de la raíz del dominio/subdominio:
```apache
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
```

**Nota:** Debes crear previamente las páginas HTML en la carpeta `/error/` de tu proyecto.

#### **4. Redirección HTTP a HTTPS**

Fuerza el uso de HTTPS para todas las peticiones:
```apache
RewriteEngine On
RewriteCond %{SERVER_PORT} 80
RewriteRule ^(.*)$ https://10.199.8.153 [R,L]
```

**Importante:** Cambia la dirección IP por la de tu servidor.

#### **Ejemplo completo de .htaccess**
```apache
# Redirección a índice personalizado
directoryindex indexProyectoCIB.php

# Redirección de errores a páginas personalizadas en carpeta error de raíz del dominio/subdominio
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
```

**Recursos adicionales:**

Para más información sobre configuraciones avanzadas de `.htaccess`, consulta:  
https://www.ionos.es/digitalguide/hosting/cuestiones-tecnicas/los-mejores-trucos-para-archivos-htaccess/

---
