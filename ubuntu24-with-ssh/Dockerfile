FROM ubuntu:24.04

# Update package lists and install OpenSSH server and sudo package
RUN apt update && apt install -y openssh-server sudo
# Create directory for SSH daemon runtime files
RUN mkdir /var/run/sshd

# Add new user 'admin' with home directory and bash shell
RUN useradd -m -s /bin/bash admin && echo "admin:123456" | chpasswd

# Grant 'admin' user passwordless sudo privileges
RUN echo "admin ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/admin && chmod 440 /etc/sudoers.d/admin

# Create .ssh directory for 'admin' user and set proper permissions
RUN mkdir -p /home/admin/.ssh && \
    echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINHANjuBxtH/kmRpXvZ2PNP0P1PajI0moNZepTUai8vc kac@kac" > /home/admin/.ssh/authorized_keys && \
    chown -R admin:admin /home/admin/.ssh && \
    chmod 700 /home/admin/.ssh && \
    chmod 600 /home/admin/.ssh/authorized_keys

# Disable root login via SSH for security
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config

# Allow only 'admin' user to login via SSH
RUN echo "AllowUsers admin" >> /etc/ssh/sshd_config

# Expose SSH port
EXPOSE 22

# Start SSH daemon in foreground
CMD ["/usr/sbin/sshd", "-D"]
