# Use the official Arch Linux image as the base
FROM archlinux:latest

# Update the system and install necessary packages
RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm openssh mariadb apache sudo

# SSH setup
RUN ssh-keygen -A
RUN echo 'root:test123' | chpasswd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# MariaDB setup
RUN mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql

# Apache setup
RUN echo "ServerName localhost" >> /etc/httpd/conf/httpd.conf

# Install supervisor
RUN pacman -S --noconfirm python-pip && \
    pip install supervisor

# Copy supervisord.conf
COPY supervisord.conf /etc/supervisord.conf

# Expose ports for the services
EXPOSE 22 80 3306

# Start supervisord to manage processes
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
