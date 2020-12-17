# VPC-NFS-Server

En el presente repositorio se describen los pasos de configuración de un NFS Server para la conexión de más de un virtual server a un mismo block storage como volumen de datos, tal como se muestra en el siguiente diagrama:

![](https://user-images.githubusercontent.com/60897075/102551130-fe3de380-408c-11eb-8cc3-59582f20ea65.png)

## Configuración del servidor NFS 

Paso 1: Instalación de nfs utils en el servidor NFS mediante el siguiente comando:

```shell
yum -y install nfs-utils
```

Paso 2: Habilitar e iniciar el NFS server service mediante los siguientes comandos:

```shell
systemctl enable nfs-server.servicesystemctl
start nfs-server.service
```
