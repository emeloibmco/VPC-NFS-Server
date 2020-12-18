# VPC-NFS-Server

En el presente repositorio se describen los pasos de configuración de un NFS Server para la conexión de más de un virtual server a un mismo block storage como volumen de datos, tal como se muestra en el siguiente diagrama:

![](https://user-images.githubusercontent.com/60897075/102551130-fe3de380-408c-11eb-8cc3-59582f20ea65.png)

## Configuración del servidor NFS 

Paso 1: Instalación de **nfs utils** en el servidor NFS mediante el siguiente comando:

```shell
yum -y install nfs-utils
```

Paso 2: Habilitar e iniciar el NFS server service mediante los siguientes comandos:

```shell
systemctl enable nfs-server.servicesystemctl
start nfs-server.service
```

Paso 3: Exportar los directorios a los cuales se desea que el cliente tenga acceso. Mediante los siguientes comandos se crea el directorio var/nfs, que para la guía va a ser el directorio compartido entre el cliente y el servidor. Dependiendo del directorio a compartir es probable que necesite cambiar su propietario para que pueda ser accedido por el cliente, para este caso puede dirigirse a la documentación del comando [chown.](https://www.servidoresadmin.com/comando-chown-en-linux/)

```shell
mkdir /var/nfs
```

Luego de esto,  para especificar los directorios a exportar modificaremos el archivo /etc/export como se muestra a continuación:

```shell
nano /etc/exports
```

Contenido del archivo:

```shell
/var/nfs 192.168.1.101 (rw, sync, no_subtree_check)
```

En el archivo anterior primero se especifica el directorio a exportar, luego la IP del cliente con la que va a ser compartida y finalmente se especifican las diferentes opciones de NFS que podemos encontrar en este [link](https://www.tecmint.com/how-to-setup-nfs-server-in-linux/).

Para hacer efectivos los cambios es necesario correr el comando:

```
exportfs -a
```

## Configuración del cliente NFS
