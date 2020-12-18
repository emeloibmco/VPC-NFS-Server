# VPC-NFS-Server

En el presente repositorio se describen los pasos de configuración de un NFS Server para la conexión de más de un virtual server a un mismo block storage como volumen de datos, tal como se muestra en el siguiente diagrama:

![](https://user-images.githubusercontent.com/60897075/102551130-fe3de380-408c-11eb-8cc3-59582f20ea65.png)

## Configuración del servidor NFS 

**Paso 1:** Instalación de **nfs utils** en el servidor NFS mediante el siguiente comando:

```shell
yum -y install nfs-utils
```

**Paso 2:** Habilitar e iniciar el NFS server service mediante los siguientes comandos:

```shell
systemctl enable nfs-server.servicesystemctl
start nfs-server.service
```

**Paso 3:** Exportar los directorios a los cuales se desea que el cliente tenga acceso. Mediante los siguientes comandos se crea el directorio **var/nfs**, que para la guía va a ser el directorio compartido entre el cliente y el servidor. Dependiendo del directorio a compartir es probable que necesite cambiar su propietario para que pueda ser accedido por el cliente, para este caso puede dirigirse a la documentación del comando [chown.](https://www.servidoresadmin.com/comando-chown-en-linux/)

```shell
mkdir /var/nfs
```

Luego de esto,  para especificar los directorios a exportar modificaremos el archivo **/etc/export** como se muestra a continuación:

```shell
nano /etc/exports
```

Contenido del archivo:

```shell
/var/nfs 192.168.1.101 (rw, sync, no_subtree_check)
```

En el archivo anterior primero se especifica el directorio a exportar, luego la IP del cliente con la que va a ser compartida y finalmente se especifican las diferentes opciones de NFS que podemos encontrar en este [link](https://www.tecmint.com/how-to-setup-nfs-server-in-linux/).

Para hacer efectivos los cambios es necesario correr el comando:

```shell
exportfs -a
```

## Configuración del cliente NFS

**Paso 1:** Instalación de **nfs utils** en el cliente NFS mediante el siguiente comando:

```shell
yum install nfs-utils
```

**Paso 2:** Crear los directorios donde se quieren montar los directorios compartidos con el servidor nfs.

```shell
mkdir -p /mnt/nfs/var/nfs
```

**Paso 3:** Montar el directorio compartido en el que se acaba de crear:

```shell
mount 192.168.1.100:/var/nfs /mnt/nfs/var/nfs
```

En el comando anterior primero se especifica la IP del servidor nfs seguido del directorio compartido y finalmente la carpeta del cliente donde se montará.

## Pruebas

Para comprobar que la configuración se hizo de manera correcta se puede crear un archivo dentro del directorio compartido ya sea desde el lado del servidor o desde el lado del cliente y debería encontrarlo en ambas máquinas. 

Si se desea añadir más de un cliente a la configuración, añada sus directorios e IPs en **/etc/export** desde el lado del servidor y siga los pasos de la guía **Configuración del cliente NFS.**

Es posible que desee exportar un file system específico, para crearlo puede seguir los pasos de este [link.](https://rm-rf.es/crear-y-eliminar-particiones-con-fdisk-en-linux/)

## Referencias

Para tener más información sobre la configuración del servidor y clientes NFS diríjase al siguiente [link.](https://www.howtoforge.com/tutorial/setting-up-an-nfs-server-and-client-on-centos-7/)

## **Autores**

IBM Cloud Tech Sales
