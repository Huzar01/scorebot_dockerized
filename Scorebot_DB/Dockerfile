# Use the official Arch Linux image as the base
FROM archlinux:latest

# Update the system and install necessary packages
RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm sudo git mariadb supervisor vim lsof systemd

# Set up MariaDB data directory
RUN mkdir -p /var/lib/mysql && \
    chown -R mysql:mysql /var/lib/mysql && \
    chmod 750 /var/lib/mysql

# Create the sbadmin user with the specified password and sudo permissions
RUN useradd -m sbadmin && \
    echo "sbadmin:test123" | chpasswd && \
    echo "sbadmin ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/sbadmin

# Clone the repository and run the setup script
RUN git clone https://github.com/Huzar01/scorebot-misc 
#&& \
#    chmod +x scorebot-misc/database.sh && \
#    ./scorebot-misc/database.sh

# Expose the MariaDB and SSH ports
EXPOSE 3306 

# Create a simple supervisord configuration
#RUN echo "[supervisord]\nnodaemon=true\n" > /etc/supervisor/conf.d/supervisord.conf

# Start supervisord to manage processes
#CMD ["/usr/bin/supervisord"]
#CMD ["mysqld_safe", "-t"]
#CMD ["tail", "-f", "/dev/null"]