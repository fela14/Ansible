# CentOS Stream 9 image for Ansible target nodes
FROM quay.io/centos/centos:stream9

# Install SSH server, sudo, and passwd
RUN yum -y update && \
    yum -y install openssh-server sudo passwd shadow-utils && \
    yum clean all

# Generate host SSH keys
RUN ssh-keygen -A

# Configure SSH daemon
RUN grep -q "^PermitRootLogin" /etc/ssh/sshd_config \
    && sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config \
    || echo "PermitRootLogin yes" >> /etc/ssh/sshd_config && \
    grep -q "^PasswordAuthentication" /etc/ssh/sshd_config \
    && sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config \
    || echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config && \
    echo "UsePAM yes" >> /etc/ssh/sshd_config && \
    echo "AllowUsers ansible" >> /etc/ssh/sshd_config

# Create ansible user, set password, sudo, and shell
RUN useradd -m -s /bin/bash ansible && \
    echo "ansible:ansible" | chpasswd && \
    usermod -aG wheel ansible && \
    echo "ansible ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers 
    
# Prepare SSH runtime directories
RUN mkdir -p /var/run/sshd && \
    mkdir -p /root/.ssh && chmod 700 /root/.ssh

# Expose SSH port
EXPOSE 22

# Run SSHD in foreground
CMD ["/usr/sbin/sshd", "-D"]

