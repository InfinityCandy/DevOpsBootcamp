FROM ubuntu

WORKDIR /usr/src/app

# Copy target cotainer files from local machine to container
COPY . .

# Install basci packages for SSH to work and NodeJS
RUN apt-get update  -y \
    && apt-get upgrade -y \ 
    && apt-get install -y \
    ssh \
    sshpass \
    sudo \
    software-properties-common \
    nodejs 

# Ansible installation and basic configuration
RUN add-apt-repository --yes --update ppa:ansible/ansible \
    && apt-get install -y ansible
RUN useradd -rm -d /home/ansible_controller -s /bin/bash -g root -G sudo -u 1001 ansible_controller
RUN echo ansible_controller:12345 | chpasswd
RUN echo "ansible_controller ALL=(ALL:ALL) NOPASSWD: ALL" |  EDITOR="tee -a"  visudo

# Container additional cofiguration
RUN mkdir -p /usr/src/app/.ssh
RUN chmod +x /usr/src/app/startup.sh

# NPM installation, packages installation and port exposition
RUN apt-get install -y npm
RUN npm install
EXPOSE 3000

CMD ["/bin/bash", "/usr/src/app/startup.sh"]