# Use the official Arch Linux image as the base
FROM archlinux:latest

# Update the system and install necessary packages
RUN pacman -Syu --noconfirm && \
    pacman -S --noconfirm openssh sudo git which gcc vim
    
# Configure SSH
#RUN mkdir /var/run/sshd && \
#    echo 'PermitRootLogin no' >> /etc/ssh/sshd_config && \
#    echo 'PasswordAuthentication yes' >> /etc/ssh/sshd_config && \
#    echo 'AllowUsers sbadmin' >> /etc/ssh/sshd_config

# Create the sbadmin user with sudo permissions
RUN useradd -m sbadmin && \
    echo "sbadmin ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/sbadmin

# Switch to the sbadmin user
USER sbadmin

# Set the working directory for sbadmin
WORKDIR /home/sbadmin

# Install Go and set the GOPATH
RUN sudo pacman -S --noconfirm go && \
    echo 'export GOPATH="$HOME/go"' >> ~/.bashrc && \
    echo 'export PATH="$PATH:$GOPATH/bin"' >> ~/.bashrc && \
    source ~/.bashrc

# Clone the Scorebot-Scoreboard repository and build the project
RUN git clone https://github.com/iDigitalFlame/Scorebot-Scoreboard.git && \
    cd Scorebot-Scoreboard && \
    bash build.sh && \
    echo '{ \
        "log": { \
            "file": "scoreboard.log", \
            "level": 2 \
        }, \
        "tick": 5, \
        "assets": "", \
        "listen": "0.0.0.0:8080", \
        "twitter": { \
            "filter": { \
                "language": [ \
                    "en" \
                ], \
                "keywords": [ \
                    "pvj", \
                    "ctf" \
                ], \
                "only_users": [], \
                "blocked_users": [], \
                "banned_words": [] \
            }, \
            "expire": 45, \
            "auth": { \
                "access_key": "", \
                "consumer_key": "", \
                "access_secret": "", \
                "consumer_secret": "" \
            } \
        }, \
        "timeout": 10, \
        "key": "", \
        "scorebot": "http://scorebot", \
        "cert": "" \
    }' > scoreboard/config.json

WORKDIR /home/sbadmin/Scorebot-Scoreboard

# Expose the SSH port
EXPOSE 8080

# Start the SSH server
#CMD ["/bin/sh"]
CMD ["bin/scoreboard", "-c", "scoreboard/config.json"]

