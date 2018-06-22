## Instalación y Post Instalación de Docker CE

1. Pre requerimientos
2. Instalación Docker Community Edition
3. Post Instalación

## Pre requerimientos

Habilitar en el SO **repos rhel-7-server-extras-rpms**

> sudo subscription-manager repos --enable=rhel-7-server-extras-rpms

En caso de requerirlo instalar **pigz**

> wget ftp://ftp.pbone.net/mirror/download.fedora.redhat.com/pub/fedora/epel/7/x86\_64/Packages/p/pigz-2.3.4-1.el7.x86\_64.rpm

```
--2018-05-29 09:36:31--  ftp://ftp.pbone.net/mirror/download.fedora.redhat.com/pub/fedora/epel/7/x86_64/Packages/p/pigz-2.3.4-1.el7.x86_64.rpm
           => “pigz-2.3.4-1.el7.x86_64.rpm”
Resolviendo ftp.pbone.net (ftp.pbone.net)... 93.179.225.212
Conectando con ftp.pbone.net (ftp.pbone.net)[93.179.225.212]:21... conectado.
Identificándose como anonymous ... ¡Dentro!
==> SYST ... hecho.   ==> PWD ... hecho.
==> TYPE I ... hecho.  ==> CWD (1) /mirror/download.fedora.redhat.com/pub/fedora/epel/7/x86_64/Packages/p ... hecho.
==> SIZE pigz-2.3.4-1.el7.x86_64.rpm ... 82616
==> PASV ... hecho.   ==> RETR pigz-2.3.4-1.el7.x86_64.rpm ... hecho.
Longitud: 82616 (81K) (probablemente)

100%[========================================================================================================>] 82 616       221KB/s   en 0.4s

2018-05-29 09:36:34 (221 KB/s) - “pigz-2.3.4-1.el7.x86_64.rpm” guardado [82616]
```

> sudo rpm -ihv pigz-2.3.4-1.el7.x86\_64.rpm

```
advertencia:pigz-2.3.4-1.el7.x86_64.rpm: EncabezadoV3 RSA/SHA256 Signature, ID de clave 352c64e5: NOKEY
Preparando...                         ################################# [100%]
Actualizando / instalando...
   1:pigz-2.3.4-1.el7                 ################################# [100%]
```

Agregar repositorio de Docker CE

> sudo wget [https://download.docker.com/linux/centos/docker-ce.repo](https://download.docker.com/linux/centos/docker-ce.repo) -O /etc/yum.repos.d/docker-ce.repo

```
--2018-05-29 09:16:43--  https://download.docker.com/linux/centos/docker-ce.repo
Resolviendo download.docker.com (download.docker.com)... 52.85.131.194, 52.85.131.233, 52.85.131.94, ...
Conectando con download.docker.com (download.docker.com)[52.85.131.194]:443... conectado.
Petición HTTP enviada, esperando respuesta... 200 OK
Longitud: 2424 (2.4K) [binary/octet-stream]
Grabando a: “/etc/yum.repos.d/docker-ce.repo”

100%[========================================================================================================>] 2 424       --.-K/s   en 0s

2018-05-29 09:16:43 (492 MB/s) - “/etc/yum.repos.d/docker-ce.repo” guardado [2424/2424]
```

## Instalación Docker Community Edition

**Instalar Docker CE **

> sudo yum install -y docker-ce

```
Complementos cargados:langpacks, product-id, search-disabled-repos, subscription-manager
Resolviendo dependencias
--> Ejecutando prueba de transacción
---> Paquete docker-ce.x86_64 0:18.03.1.ce-1.el7.centos debe ser instalado
--> Procesando dependencias: container-selinux >= 2.9 para el paquete: docker-ce-18.03.1.ce-1.el7.centos.x86_64
--> Procesando dependencias: libltdl.so.7()(64bit) para el paquete: docker-ce-18.03.1.ce-1.el7.centos.x86_64
--> Ejecutando prueba de transacción
---> Paquete container-selinux.noarch 2:2.55-1.el7 debe ser instalado
--> Procesando dependencias: policycoreutils-python para el paquete: 2:container-selinux-2.55-1.el7.noarch
---> Paquete libtool-ltdl.x86_64 0:2.4.2-22.el7_3 debe ser instalado
--> Ejecutando prueba de transacción
---> Paquete policycoreutils-python.x86_64 0:2.5-22.el7 debe ser instalado
--> Procesando dependencias: audit-libs-python >= 2.1.3-4 para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: libsemanage-python >= 2.5-9 para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: setools-libs >= 3.3.8-2 para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: checkpolicy para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: libapol.so.4(VERS_4.0)(64bit) para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: libqpol.so.1(VERS_1.2)(64bit) para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: libqpol.so.1(VERS_1.4)(64bit) para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: python-IPy para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: libapol.so.4()(64bit) para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Procesando dependencias: libqpol.so.1()(64bit) para el paquete: policycoreutils-python-2.5-22.el7.x86_64
--> Ejecutando prueba de transacción
---> Paquete audit-libs-python.x86_64 0:2.8.1-3.el7 debe ser instalado
---> Paquete checkpolicy.x86_64 0:2.5-6.el7 debe ser instalado
---> Paquete libsemanage-python.x86_64 0:2.5-11.el7 debe ser instalado
---> Paquete python-IPy.noarch 0:0.75-6....
```

**Problemas de Instalación por Selinux**

En algunos casos puede ocurrir un error por sincronización con la suscripción de Red Hat, y es común tener problemas en la instalación de Docker por SElinux, si esto ocurre tendremos que instalar SELinux en nuestro SO con los siguientes comandos:

> sudo yum-config-manager --save --setopt=rhel-7-server-extras-rpms.skip\_if\_unavailable=true
>
> sudo yum install -y [http://mirror.centos.org/centos/7/extras/x86\_64/Packages/container-selinux-2.55-1.el7.noarch.rpm](http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.55-1.el7.noarch.rpm)

Y continuar con la instalación de Docker

> sudo yum install -y docker-ce

Para comprobar el estado del servicio de Docker será necesario el siguiente comando:

> sudo systemctl status docker

Si el servicio no está activo, será necesario activarlo:

> sudo systemctl start docker

## Post Instalación

Manejar Docker como _**no-root user**_

Crear el grupo **docker** si no existe

> sudo groupadd docker

Añadir el usuario al grupo

> sudo usermod -aG docker $USER

**Volver a iniciar sesión** con el usuario y probar el siguiente comando:

> docker ps



