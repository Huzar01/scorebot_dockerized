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

# Set the working directory
WORKDIR /root

# Run bash shell as the default command
CMD ["/bin/bash"]