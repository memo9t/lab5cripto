FROM ubuntu:22.10

# Usar repositorios antiguos
RUN sed -i 's|archive.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list && \
    sed -i 's|security.ubuntu.com|old-releases.ubuntu.com|g' /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y openssh-client openssh-server sudo iproute2 iputils-ping net-tools && \
    apt-get clean

# Crear usuario 'prueba' con contraseña 'prueba'
RUN useradd -m prueba && echo "prueba:prueba" | chpasswd && adduser prueba sudo

# Crear carpeta de SSH y configurar acceso
RUN mkdir -p /var/run/sshd && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config && \
    sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]
