# Laboratorio Final - Docker, SSH y Criptografía Asimétrica

Este repositorio contiene el desarrollo del laboratorio final cuyo objetivo es comprender el uso de **Docker** y el protocolo **SSH**, enfocado en la **criptografía asimétrica** y **firmas digitales**.

##  Objetivo del Laboratorio

Este laboratorio busca simular distintos entornos con versiones variadas de Ubuntu, conectados entre sí mediante SSH, permitiendo experimentar con:

- Configuración y despliegue de contenedores Docker personalizados.
- Instalación y configuración del cliente y servidor SSH.
- Acceso remoto mediante autenticación basada en contraseña.
- Fundamentos de la criptografía asimétrica aplicada a conexiones seguras.

---

##  Escenario

Se crean **4 contenedores Docker** con diferentes versiones de Ubuntu:

| Contenedor | Versión de Ubuntu | Rol   |
|------------|-------------------|--------|
| C1         | Ubuntu 16.10      | Cliente |
| C2         | Ubuntu 18.10      | Cliente |
| C3         | Ubuntu 20.10      | Cliente |
| C4         | Ubuntu 22.10      | Cliente y Servidor (S1) |

- **S1** es el servidor SSH y corresponde al contenedor **C4**.

---

##  Configuración

### 1. Dockerfile

Para cada contenedor se crea un `Dockerfile` que incluye:

- Instalación del cliente OpenSSH (`openssh-client`).
- Instalación del servidor SSH (`openssh-server`) **solo en S1 (C4)**.
- Creación del usuario `prueba` con contraseña `prueba` en S1.
- Configuración del servicio `sshd` para permitir accesos remotos.

### 2. Construcción de Contenedores

Desde la raíz del proyecto:

```bash
# Crear las imágenes 
cd C1 && sudo docker build -t c1 . && cd ..
cd C2 && sudo docker build -t c2 . && cd ..
cd C3 && sudo docker build -t c3 . && cd ..
cd C4 && sudo docker build -t c4 . && cd ..
```
### 3.Crear red Docker para que todos los contenedores se vean

```bash
sudo docker network create ssh-net
```
### 4. Ejecutar contenedores conectados a la misma red

```bash
# Servidor SSH
docker run -dit --name s1 --hostname s1 --network ssh-net -p 2222:22 c4

# Clientes
docker run -dit --name c1 --hostname c1 --network ssh-net c1
docker run -dit --name c2 --hostname c2 --network ssh-net c2
docker run -dit --name c3 --hostname c3 --network ssh-net c3
```
### 5. Probar conexión SSH desde cada cliente

```bash
# Desde C1 (en otra terminal o con docker exec)
sudo docker exec -it c1 bash
ssh prueba@s1
# contraseña: prueba

# Igual para C2
sudo docker exec -it c2 bash
ssh prueba@s1

# Igual para C3
sudo docker exec -it c3 bash
ssh prueba@s1

# Igual para C4
sudo docker exec -it s1 bash
ssh prueba@localhost
```


