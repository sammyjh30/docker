FROM ubuntu:18.04

ARG DEBIAN_FRONTEND=noninteractive

ENV TERM xterm

RUN apt-get update && apt-get install -y \
    xterm \
    curl \
    openssh-server \               
    ca-certificates \
    postfix \
    tzdata

RUN curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash

RUN apt-get install gitlab-ce

ARG ip

RUN echo 'external_url "https://'${ip}'"' >> /etc/gitlab/gitlab.rb
RUN echo "letsencrypt['enable'] = false" >> /etc/gitlab/gitlab.rb
RUN echo "prometheus_monitoring['enable'] = false" >> /etc/gitlab/gitlab.rb
RUN echo "grafana['enable'] = false" >> /etc/gitlab/gitlab.rb
RUN echo "unicorn['worker_timeout'] = 300" >> /etc/gitlab/gitlab.rb
RUN echo "gitlab_rails['gitlab_shell_ssh_port'] = 8022" >> /etc/gitlab/gitlab.rb
RUN mkdir -p /opt/gitlab/sv/sshd/supervise \
    && mkfifo /opt/gitlab/sv/sshd/supervise/ok \
    && printf "#!/bin/sh\nexec 2>&1\numask 077\nexec /usr/sbin/sshd -D" > /opt/gitlab/sv/sshd/run \
    && chmod a+x /opt/gitlab/sv/sshd/run \
    && ln -s /opt/gitlab/sv/sshd /opt/gitlab/service \
    && mkdir -p /var/run/sshd
RUN echo "UseDNS no" >> /etc/ssh/sshd_config
RUN echo "Port 8022" >> /etc/ssh/sshd_config
RUN echo "Port 8022" >> /etc/ssh/ssh_config

# 1. Create the SSH directory.
# 2. Populate the private key file.
# 3. Set the required permissions.
# 4. Add github to our list of known hosts for ssh.
RUN mkdir -p /root/.ssh/ && \
    echo "$SSH_KEY" > /root/.ssh/id_rsa && \
    chmod -R 600 /root/.ssh/ && \
    ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts

WORKDIR /etc/gitlab/ssl

RUN openssl req -x509 -sha256 -newkey rsa:2048 -nodes \
    -keyout ${ip}.key -days 365 -out ${ip}.crt -subj '/CN='${ip}

WORKDIR /

EXPOSE 443 8022

CMD (service ssh restart) && (opt/gitlab/embedded/bin/runsvdir-start &) && (sysctl vm.overcommit_memory=1 &) && gitlab-ctl reconfigure && gitlab-ctl tail

# FROM debian:10.1

# ENV TERM=xterm

# RUN apt-get update && \
#     apt-get install -y xterm curl openssh-server ca-certificates postfix
# RUN curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
# RUN apt-get install -y gitlab-ce
# RUN sed -i "s/external_url 'https://'${ip}'/g" /etc/gitlab/gitlab.rb
# RUN sed -i "s/# grafana\['enable'\] = true/grafana['enable'] = false/g" /etc/gitlab/gitlab.rb
# RUN sed -i "s/unicorn\['worker_timeout'\] = 60/unicorn\['worker_timeout'\] = 300/g" /etc/gitlab/gitlab.rb
# RUN sed -i "s/gitlab_rails\['gitlab_shell_ssh_port'\] = 8022/g" /etc/gitlab/gitlab.rb

# EXPOSE 22 80 443

# ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && \
#            gitlab-ctl reconfigure && tail -f /dev/null

# docker build -t ex03 ./
# docker build -t --build-arg ip=$(docker-machine ip Char) ex03 ./
# docker run -it --shm-size=4g --restart=on-failure -p 8080:80 -p 8022:22 -p 8443:443 --privileged --name gls ex03

# git clone http://192.168.99.101:8080/root/gitlab_test_sam.git

# ssh -T -p 8080 -i ttester.pub git@192.168.99.101

# https://github.com/g0dReVeN/WTC-docker-1/blob/master/01_dockerfiles/ex03/Dockerfile