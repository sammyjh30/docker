FROM debian:10.1

ENV TERM=xterm
RUN apt-get update && \
    apt-get install -y xterm curl openssh-server ca-certificates postfix
RUN curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-get install -y gitlab-ce
RUN sed -i "s/unicorn\['worker_timeout'\] = 60/unicorn\['worker_timeout'\] = 300/g" /etc/gitlab/gitlab.rb

EXPOSE 22 80 443
ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && \
           gitlab-ctl reconfigure

# 
# docker build -t gitlab-ce12.4.0 ./ && docker run --name gitlab-ce12.4.0 -itd --shm-size=4g --restart=on-failure  -p 8080:80 -p 8022:22 -p 8443:443 --privileged gitlab-ce12.4.0

# docker build -t gitlab-ce12.4.0 ./
# docker run --name gitlab-ce12.4.0 -itd --shm-size=4g --restart=on-failure  -p 8080:80 -p 8022:22 -p 8443:443 --privileged gitlab-ce12.4.0
# docker run -itd --restart=on-failure -p 8080:80 -p 8022:22 -p 8443:443 --privileged gitlab-ce12.4.0