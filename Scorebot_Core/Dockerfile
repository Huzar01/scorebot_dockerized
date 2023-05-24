# Use the official ArchLinux base image
FROM archlinux:latest

# Update system and install required packages
RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm openssh sudo git screen which gcc binutils make pkg-config fakeroot

# Configure SSHD and create a user with sudo rights
RUN systemctl enable sshd && \
    echo -e "test123\ntest123" | passwd && \
    useradd -m -s /bin/bash sbadmin && \
    echo -e "test123\ntest123" | passwd sbadmin && \
    echo 'sbadmin ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/sbadmin

# Clone mod_wsgi repository and build it
RUN git clone https://aur.archlinux.org/mod_wsgi.git && \
    chown -R sbadmin:sbadmin mod_wsgi && \
    cd mod_wsgi && \
    sudo -u sbadmin makepkg -si --noconfirm

# Add kludge configuration and run setup.sh script
RUN git clone https://github.com/quicksandcodes/scorebot-misc/ && \
    cd scorebot-misc && \ 
    chmod +x setup.sh && \
    sudo -u sbadmin ./setup.sh | tee setup.out

# Configure and enable services
RUN systemctl enable scorebot && \
    systemctl enable httpd

# Expose SSH and HTTP ports
EXPOSE 22 80

# Start SSH and HTTP services
CMD ["sh", "-c", "systemctl start sshd; systemctl start httpd; exec /bin/bash"]
