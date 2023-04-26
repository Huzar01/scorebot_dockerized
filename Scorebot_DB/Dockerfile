# Use the official MariaDB image as the base
FROM mariadb:latest

# Set environment variables for MariaDB configurations
ENV MYSQL_ROOT_PASSWORD=test123
ENV MYSQL_USER=sbadmin
ENV MYSQL_PASSWORD=test321
ENV MYSQL_DATABASE=scorebot

# Install necessary packages and create the sbadmin user
RUN apt-get update && \
    apt-get install -y sudo && \
    useradd -m sbadmin && \
    echo "sbadmin:test123" | chpasswd && \
    echo "sbadmin ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/sbadmin

# Configure SSH and enable the service
#RUN systemctl enable ssh && \
#    systemctl start ssh

# Clone the repository and run the setup script with the specified options
#RUN git clone https://github.com/quicksandcodes/scorebot-misc.git && \
#    chmod +x scorebot-misc/setup.sh && \
#    echo -e "2\ntest123\ntest321\ntest321\n10.150.0.190" | ./scorebot-misc/setup.sh

# Expose the MariaDB port
EXPOSE 3306

# Start the MariaDB server
CMD ["mysqld"]