- [1. Servidor de Desarrollo](#1-servidor-de-desarrollo)
  - [1.1 Ubuntu Server 26.04.1 LTS](#11-ubuntu-server-26041-lts)
    - [Configuración inicial](#configuración-inicial)
    - [Nombre y configuración de red](#nombre-y-configuración-de-red)
    - [Actualizar el sistema](#actualizar-el-sistema)
    - [Configuración de fecha y hora](#configuración-de-fecha-y-hora)
    - [Cuentas administradoras](#cuentas-administradoras)
    - [Habilitar cortafuegos](#habilitar-cortafuegos)
    - [Instalar antivirus](#instalar-antivirus)
    - [Comprobar conexión](#comprobar-conexión)
  - [1.2 Apache HTTP](#12-apache-http)
    - [Instalación](#instalación)
    - [Verficación del servicio](#verficación-del-servicio)
    - [Permisos y usuarios](#permisos-y-usuarios)
    - [Protocolo HTTPS](#protocolo-https)
    - [Instalación](#instalación-1)
    - [Configuración del php.ini para un entorno de desarrollo.](#configuración-del-phpini-para-un-entorno-de-desarrollo)
  - [1.4 MariaDB](#14-mariadb)
      - [Instalación y configuración de MariaDB](#instalación-y-configuración-de-mariadb)
    - [Consola de MariaDB](#consola-de-mariadb)
    - [Creación de un usuario administrador](#creación-de-un-usuario-administrador)
  - [1.5 PHPMyadmin](#15-phpmyadmin)
    - [Instalación](#instalación-2)
    


## 1. Servidor de Desarrollo

### 1.1 Ubuntu Server 26.04.1 LTS

#### Configuración inicial
**Configuración principal**
> **Nombre de la máquina**: acp-used\
> **Memoria RAM**: 2G\
> **Particiones**: 150G(/) y resto (350GB) (/var)\
> **Configuración de red interface**: xxxx \
> **Dirección IP** :10.199.8.153/22\
> **GW**: 10.199.8.1\
> **DNS**: 10.151.123.21 o 10.151.126.21

Para comprobar todos estos valores debemos usar los siguientes comandos:
```bash
hostname

lsblk
sudo fdisk -l /dev/sda                          # Comprobar las distintas particiones del disco, su tamaño y su raíz
```
#### Nombre y configuración de red
**Configuración de red**
Antes de realizar cualquier cambio en la configuración mostramos la ip que tenemos y el dns de nuestra red mediante estos comandos.
```bash
ip a
```
Editar el fichero de configuración del interface de red  **/etc/netplan**.

```bash
# This is the network config written by 'subiquity'
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 10.199.8.153/22
      routes:
        - to: default
          via: 10.199.8.1
      nameservers:
        addresses: [10.151.123.21, 10.151.126.21]
        search: [educa.jcyl.es]
````

Para terminar, debemos realizar el siguiente comando:
```bash
sudo netplan apply              # Aplicamos la configuración de red guardada en el archivo enp0s3.yaml.
```
#### Actualizar el sistema
Antes de realizar una instalación nueva o al encender nuestra máquina virtual se debe actualizar el sistema
y sus aplicaciones.

```bash
sudo apt update                 # Actualiza el sistema y aplicaciones
sudo apt upgrade                # Actualiza los paquetes
```
#### Configuración de fecha y hora
Para comprobar la fecha y la hora, y modificarlas, usaremos los siguientes comandos: 
```bash
date                                                # Comprobar la fecha y hora
# Comprobar la hora y fechar en formato: día YYYY-MM-DD HH:MM:SS 
# También muestra la zona horaría en la que está trabajando el sistema.
timedatectl

# Para modificar la zona horaria debemos usar el siguiente comando:
sudo timedatectl set-timezone Europe/Madrid         # En caso de querer poner la zona horaria de Madrid UTG+1.
```

#### Cuentas administradoras 



> root(inicio)\
> miadmin/paso\  
> miadmin2/paso

Apache ya nos crea (durante la instalación principal) una cuenta administradora a la cual podemos introducirle un nombre y una contraseña(miadmin).

Una vez configurada la red y verificado el servicio, creamos el usuario miadmin2:

```bash
sudo adduser miadmin2                   #Creamos el usuario con contraseña paso e ignoramos el resto de datos que nos piden
```

Una vez creado el usuario tenemos que darle privilegios de sudo, es decir, meterle en el grupo sudoers para que pueda hacer ciertos comandos:

```bash
sudo usermod -aG sudo miadmin2          
# Meter al usuario miadmin2 en el grupo sudo sin quitarle del resto de grupos que pertenece y -G indica los grupos suplementarios a los que quieres añadir el usuario.
# Ahora lo que tenemos que hacer es el meter a miadmin2 en los mismos grupos que miadmin usando este comando y listando
  con cat /etc/group | grep miadmin

**Comandos recomendados**
su nombreusuario                        # Inicia sesión en el usuario indicado.
exit                                    # Cierra la sesión actual.
```
#### Habilitar cortafuegos
**UFW**\
Para comprobar el estado del cortafuegos y saber si está activado o desactivado, se debe  usar el siguiente comando:

```bash
sudo systemctl status ufw                           # Mostrar el status del cortafuegos.
sudo systemctl start | restart ufw                    # Arrancar el cortafuegos.
```

De la misma manera podemos comprobar el SSH:

```bash
sudo systemctl status ssh                           # Mostrar el status del servicio SSH.
sudo systemctl start|restart ssh                    # Arrancar el servicio SSH.

# A mayores tenemos que comprobar el puerto del cortafuegos ufw 22.
sudo ufw status numbered

# Normalmente va a estar activo el puerto 22 y el puerto 22 v6. Éste último hay que borrarlo.
sudo ufw delete "numeropuerto"
```

#### Instalar antivirus
Se actualiza el servidor
```bash
sudo apt update
sudo apt upgrade -y
```

Se intala el antivirus clamav

```bash
sudo apt install clamav clamav-daemon -y
```

Se actualiza la base de datos del virus:
Pimero se detiene el servicio
```bash
sudo systemctl stop clamav-freshclam
```

y se actualiza la base de datos manualmente
```bash
sudo freshclam
```

Se inicia y habilita para el arranque automatico
```bash
sudo systemctl start clamav-freshclam
sudo systemctl enable clamav-freshclam
```

Se verifica el servicio de actualización de definiciones de virus esté activo:
```bash
systemctl status clamav-freshclam
```


Para saber la version del antivirus
```bash
clamscan --version
```

#### Comprobar conexión

**SSH**\
El protocolo de red SSH permite controlar y modificar servidores remotos de manera segura a través de Internet.

En el momento que estamos instalando el sistema operativo Ubuntu Server 26.04.1 LTS el instalador nos pregunta si queremos el servicio SSH.
Por tanto tenemos dos opciones para obtener el servicio SSH en nuestra máquina:

**1ª Opción:** preinstalarlo durante la instalación del sistema operativo.

**2ª Opción:** usar el siguiente comando:
```bash
# Actualizamos el sistema operativo.
sudo apt update

# Instalamos el servicio SSH.
sudo apt install openssh-server -y
```

En cualquiera de los dos casos debemos de comprobar que se ha instalado el servicio correctamente:
```bash
sudo systemctl status ssh

# En caso de necesitar iniciarlo o reiniciarlo tenemos este comando
sudo systemctl [start|restart] ssh

# Para que el servicio se inicie cada vez que se encienda la máquina tenemos este comando
sudo systemctl enable ssh
```

Conexión SSH
Para conectarnos con la máquina con SSH deberemos abrir la consola de comandos en un dispositivo de la misma red
y escribir el siguiente comando:
```bash
ssh usuario@ip
```

Nos pedirá la contraseña del usuario y nos iniciará sesión. En ese momento podremos gestionar en remoto desde esa consola.
Hay que tener en cuenta que los límites de que se puede hacer los marcan los privilegios que tenga el usuario conectado.

### 1.2 Apache HTTP

#### Instalación
Para instalar un servidor web vamos a descargar y configurar Apache. El primer paso es actualizar el SO y 
también los paquetes instalados. A continuación instalamos Apache2 y abrimos el puerto 80 que es el utilizados 
por Apache. Al abrir el puerto se abrirá tanto el normal como el (v6) y, por recomendación de seguridad, lo borraremos.

```bash

sudo apt update                         #Actualizamos OS
sudo apt upgrade                        #Actualizamos paquetes instalados

sudo apt install apache2                #Instalamos Apache2 en la máquina
  
sudo ufw allow 80                       #Habilitamos el puerto 80 desde el cortafuegos.

sudo ufw status numbered                
sudo ufw delete 'numeropuerto'          #Borramos el puerto 80 (v6) usando su número de indetificación

```
#### Verficación del servicio

Para verificar que Apache se ha instalado y está funcionando correctamente tenemos dos formas: entrando a un 
navegador desde el ordenador anfitrión y buscando la página con al dirección IP de la máquina. Si aparece una 
página como la de abajo es que Apache esta funcionando correctamente y que ambas máquinas están en la misma red. 
La otra forma es mediante los siguientes comandos:

```bash

sudo service apache2 {opcion}
sudo systemctl {opcion} apache2

```

#### Permisos y usuarios
Ahora vamos a crear el usuario "operadoweb" que será el encargado de subir archivos al servidor. Solo podrá tener acceso a su carpeta ráiz
que es /var/www/html y pertenecerá al grupo www-data

```bash
sudo adduser --home /var/www/html --ingroup www-data --shell /bin/bash operadorweb

# Ahora debemos cambiar el dueño de la carpeta /var/www/html para que pertenezca a operadorweb
sudo chown -R operadorweb:www-data /var/www/html            # Cambia el propietario y el grupo del directorio indicado.
sudo chmod -R 775 /var/www/html                             # Cambia los permisos del usuario propietario, del grupo y del resto de propieatarios.

# Para comprobar que los cambios han surgido efecto debemos hacer lo siguiente:
ls -al /var/www/html

drwxrwxr-x 2 operadorweb www-data  4096 oct  9 10:30 .
drwxr-xr-x 3 root        root      4096 oct  9 10:30 ..
-rwxrwxr-x 1 operadorweb www-data 10671 oct  9 10:30 index.html

```
#### Protocolo HTTPS
Es la versión segura del protocolo HTTP, el cual cifra la comunicación entre el navegador y el servidor, 
garantizando confidencialidad, integridad y autenticación de los datos.

**Instalación**
Generar clave privada SSL
```bash
# Habilitamos el módulo ssl
sudo a2enmod ssl

# Generamos la solicitud de certificado.
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/acp-used.key -out /etc/ssl/certs/acp-used.crt

# A continuación vamos a introducir los datos del certificado:
Country Name: ES
State or Province Name: Zamora #opcional
Locality Name: Benavente
Organization Name: Instituto
Organizational Unit Name: Informatica #opcional
Common Name: acp-used
Email Address: alvaro.calper@educa.jcyl.es #opcional

# Para comprobar que se ha creado correctamente.
sudo ls /etc/ssl/certs | grep acp-used
sudo ls /etc/ssl/private | grep acp-used

# Reiniciamos el servicio apache
sudo systemctl restart apache2

# Nos situamos en el directorio /etc/apache2/sites-available y hacemos una copia del archivo default-ssl.conf
cd /etc/apache2/sites-available
sudo cp default-ssl.conf acp-used.conf

# Modificamos el archivo copiado acp-used.conf con las siguientes líneas:

SSLCertificateFile      /etc/ssl/certs/acp-used.crt
SSLCertificateKeyFile   /etc/ssl/private/acp-used.key

# Activamos el archivo de configuración modificado y reinicamos el servicio apache.
sudo a2ensite acp-used.conf
sudo systemctl restart apache2

# También debemos activar el puerto 443 en el firewall y borrar el v6.
sudo ufw allow 443
sudo ufw status numbered # Comprobar el número del puerto 443(v6)
sudo ufw delete (nº)
```

**Redirección de http a https**     
Se debe modificar el archivo 000-default.conf
``` bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Dentro del bloque de VirtualHost 80 se debe poner la siguiente linea
``` bash
Redirect permanent / https://10.199.8.153/

sudo systemctl restart apache2 # Reiniciamos Apache2
```
Se guarda el archivo

**Consejo**
Al entrar con un navegador al servidor, nos indica que no es seguro debido a que la autoridad certificadora no es segura.
El certificado es autofirmado por el usuario, en este caso yo, y no es de confianza para el navegador. Esto se puede se puede resolver
de dos formas.
> 1. Pedir a una autoridad certificadora que firme el certificado para que así sea reconocido por el navegador y sea seguro.\
> 2. Introducir como autoridad certificadora a tu propio usuario en tu dispositivo. No supone un riesgo ya que te estás dando confianza
> a ti mismo.
>
> ### 1.3 Ejecución PHP con PHP-FPM
FPM (FastCGI Process Manager) es un servidor de aplicaciones PHP que se encarga de interpretar código PHP.

#### Instalación
Una vez actualizado el sistema y mejorado los paquetes (update y upgrade) debemos de realizar los siguientes pasos:
```bash
# Comprobamos que apache está instalado y activo.
sudo systectl status apache2

# A continuación instalamos PHP y PHP8.3-FPM con la versión 8.3
sudo apt install php8.5-fpm php8.5
```

En caso de no haber salido error al reiniciar, creamos en nuestro directorio ráiz del servidor un info.php y le introducimos lo siguiente:
```bash
<?php
phpinfo();
?>
```
Entramos en nuestra página principal y escribimos en la barra de busqueda:
http://direccionip/info.php. Nos debería de salir una página como esta.

Por último activamos (o comprobamos que esta activado) y se recarga apache2:
```bash
sudo a2enconf php8.5-fpm
sudo systemctl restart apache2

# Si se quisiera desactivar el fichero de configuración
sudo a2disconf php8.5-fpm
```

#### Configuración del php.ini para un entorno de desarrollo.
Primero se hace una copia del archivo.  
```bash
cd /etc/php/8.5/fpm/
sudo cp php.ini php.ini.bk2026
sudo nano php.ini
```

* Configuración PHP por Entorno

Esta tabla compara las configuraciones de PHP para los entornos de **Desarrollo** y **Producción** en las secciones **General** y **Errores**.

| Directiva | DESARROLLO | PRODUCCIÓN |
| :--- | :--- | :--- |
| **GENERAL** | | |
| `file-uploads` | `On` | `On` |
| `allow-url_fopen` | `On` | `Off` |
| `memory_limit` | `256M` | `256M` |
| `upload_max_filesize` | `100M` | `100M` |
| `max_execution_time` | `360` | `360` |
| `date.timezone` | `Europe/Madrid` | `Europe/Madrid` |
| **ERRORES** | | |
| `display_errors` | `On` | `Off` |
| `error_reporting` | `E_ALL` | `E_ALL & ~E_NOTICE` |
| `display_startup_errors` | `On` | `Off` |
| `log_errors` | `On` | `On` |
| `error_log` | `/var/log/php_errors.log` | - |

---

La configuración que vamos a aplicar en nuestro servicio de PHP es el siguiente:
> display_errors: On
> display_startup: On
> memory_limit: 256M

El display_errors sirve para mostrar los errores de ejecución en la salida (navegador
o consola) y el display_startup_errors para controlar los errores que surgen durante la
ejecución PHP.

El límite de memoria o memory_limit en el archivo de configuración sirve para establecer
un límite máximo de memoria que puede usar un archivo PHP.

Una vez explicado vamos a ir paso por paso detallando como aplicar esta configuración:

Una vez instalado PHP y comprobado que nos funciona el info.php entramos en el directorio
/etc/php/8.5/fpm donde encontraremos el archivo de configuración php.ini.
```bash
cd /etc/php/8.5/fpm

# Listamos el contenido y encontramos el archivo de configuración php.ini. Lo copiamos.
sudo cp php.ini php.ini.backup

#Modificamos el archivo php.ini cambiando los valores de display_errors, display_startup_errors
y memory_limit con los valores de On, On y 256M. Puedes user ctrl+w para buscar palabras en 
el editor.
sudo nano php.ini

#Una vez modificado y guardado, reiniciamos el servicio PHP.
sudo systemctl restart php8.5-fpm
```

### 1.4 MariaDB
El gestor de base de datos que hemos escogido, compatible con PHP, es MariaDB.
MariaDB es un sistema de gestión de bases de datos relacional, muy similar a MySQL, permitiendo almacenar, organizar y acceder a información mediante el lenguaje SQL.
A continuación detallaremos el proceso de instalación y puesta en marcha de este servicio.

##### Instalación y configuración de MariaDB

```bash
# Actualizamos el OS
sudo apt update

# Instalamos MariaDB
sudo apt install mariadb-server -y

# Una vez instalado comprobamos la version de MariaDB instalada.
mariadb --version
```

**Configuración**\
Una vez instalado el servicio de MariaDB nos vamos a la configuración de esta misma.

```bash
# Nos dirigimos a este archivo y lo modificamos /etc/mysql/mariadb.conf.d/50-server.cnf
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf

# Modificamos bind-address = 127.0.0.1 por: 0.0.0.0 y guardamos
# Reiniciamos el servicio
sudo systemctl restart mariadb

# Aunque no nos de error al reiniciar debemos hacer una comprobación para estar seguros de que el proceso está en ejecución.
sudo ss -punta | grep mariadb

# Nos debe salir una linea así: tcp LISTEN 0 80 0.0.0.0:3306 0.0.0.0:* users:(("mariadb",pid=7865,fd=22))
```

Con esto ya tenemos un servidor sql trabajando correctamente en nuestra máquina ubuntu server.
Para iniciar el servicio en modo comando, nos iniciará sin autenticar ningún usuario. 

* Comandos útiles del servicio

| Acción                         | Comando                          | Descripción                                                  |
| ------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| Iniciar el servicio            | `sudo systemctl start mariadb`   | Inicia el servidor MariaDB.                                  |
| Detener el servicio            | `sudo systemctl stop mariadb`    | Detiene el servidor MariaDB.                                 |
| Reiniciar el servicio          | `sudo systemctl restart mariadb` | Reinicia el servidor.                                        |
| Ver estado del servicio        | `sudo systemctl status mariadb`  | Muestra si el servidor está activo o inactivo.               |
| Habilitar inicio automático    | `sudo systemctl enable mariadb`  | Configura el servicio para iniciarse al arrancar el sistema. |
| Deshabilitar inicio automático | `sudo systemctl disable mariadb` | Evita que el servicio se inicie automáticamente.             |
| Ver versión instalada          | `mariadb --version`              | Muestra la versión actual de MariaDB instalada.              |

#### Consola de MariaDB
Se entra al cliente:

```bash
sudo mariadb
```

Luego se ejecuta:

```sql
SHOW VARIABLES LIKE 'port';
```

Resultado esperado:

| Variable_name | Value |
| ------------- | ----- |
| port          | 3306  |

#### Creación de un usuario administrador
En sistemas Ubuntu con MariaDB 10.3, el usuario **root** se autentica mediante el complemento **unix_socket** por defecto, en lugar de una contraseña.
Esto ofrece mayor seguridad, pero puede complicar el acceso desde programas externos (p. ej., phpMyAdmin).

> **No se recomienda modificar la cuenta root.**
> En su lugar, crea una cuenta administrativa independiente para autenticación con contraseña.

Se abre el cliente de MariaDB:

```bash
sudo mariadb
```

Luego se crea un nuevo usuario con privilegios de root:

```sql
CREATE USER 'adminsql'@'%' IDENTIFIED BY 'paso';
GRANT ALL PRIVILEGES ON *.* TO 'adminsql'@'%' WITH GRANT OPTION;
```

O también se puede usar:

```sql
GRANT ALL ON *.* TO 'adminsql'@'%' IDENTIFIED BY 'paso' WITH GRANT OPTION;
```

 Se puede listar todos los usuarios y sus hosts:

```sql
SELECT User, Host FROM mysql.user;
```

Prueba de conexión con el nuevo usuario

```bash
mariadb -u adminsql -p 
```

---

* Asegurar el servidor MariaDB

Se ejecuta el script de seguridad:

```bash
sudo mariadb-secure-installation
```

Este asistente te permitirá:

* Configurar una contraseña fuerte para root.
* Eliminar usuarios anónimos.
* Deshabilitar el inicio de sesión remoto del root.
* Eliminar bases de datos de prueba.
* Recargar las tablas de privilegios.

* Pasos del asistente

1. Pulsa **Enter** si no hay contraseña de root definida.
2. Define una **contraseña segura** para el usuario root.
3. Elimina el usuario anónimo (**Sí**).
4. Desactiva el acceso remoto del usuario root (**Sí**).
5. Elimina la base de datos de prueba (**Sí**).
6. Recarga los privilegios (**Sí**).

### 1.5 PHPMyadmin

#### Instalación

* Se actualiza el servidor
```bash
sudo apt update
sudo apt upgrade
```
* Se instala phpMyadmin
```bash
sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```
Para comprobar que funciona pega esto en el navegador cambiando IP por la IP del servidor http://IP/phpmyadmin

**Conexión con NetBeans**\
Ve a la pestaña services, click derecho en databases>New connection elige mariadb y pon el driver que se puede descargar aquí
https://dbschema.com/jdbc-driver/mariadb.html?srsltid=AU7gw4UWnA-b2npz9c4sMytfK6tYh_UTAg6pUr4ddLELS1TDDtim9ulA

Al dar a next en el host pones la ip del servidor y en el user name y password pones los datos de adminsql

Debes permitir el puerto 3306 en el cortafuegos y borras el V6
``` bash 
sudo ufw allow 3306
```
Das a next 2 veces y le das el nombre que quieras a la conexión

**Conexión con Visual Studio Code**\
Debes instalar la extensión de MySQL y se añadirá una nueva pestaña lateral, haz click en esa pestaña y elige la opción de MariaDB.   
Rellena los campos y ya está.   
En el campo Group puedes poner lo que quieras, es para organizar a tu gusto la disposición de las conexiones.   