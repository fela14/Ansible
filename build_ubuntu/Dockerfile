# Dockerfile
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y openssh-server sudo && \
    mkdir /var/run/sshd && \
    echo "root:root" | chpasswd && \
    echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
    sed -i 's/UsePAM no/UsePAM yes/' /etc/ssh/sshd_config || echo 'UsePAM yes' >> /etc/ssh/sshd_config && \ 
    echo 'AllowUsers ansible' >> /etc/ssh/sshd_config
# Create non-root user for Ansible
RUN useradd -ms /bin/bash ansible && \
    echo "ansible:ansible" | chpasswd && \
    adduser ansible sudo

EXPOSE 22

CMD ["/usr/sbin/sshd", "-D"]


