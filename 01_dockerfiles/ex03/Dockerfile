FROM debian:10.1

ENV TERM=xterm

RUN apt-get update && \
    apt-get install -y xterm curl openssh-server ca-certificates postfix
RUN curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-get install -y gitlab-ce
RUN echo "external_url 'https://'${ip}'" /etc/gitlab/gitlab.rb
RUN echo "# grafana\['enable'\] = true/grafana['enable'] = false" /etc/gitlab/gitlab.rb
RUN echo "unicorn\['worker_timeout'\] = 60/unicorn\['worker_timeout'\] = 300" /etc/gitlab/gitlab.rb
RUN echo "gitlab_rails\['gitlab_shell_ssh_port'\] = 8022" /etc/gitlab/gitlab.rb

EXPOSE 22 80 443

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &) && \
           gitlab-ctl reconfigure && tail -f /dev/null

# docker build -t ex03 ./
# docker run -it --shm-size=4g --restart=on-failure -p 8080:80 -p 8022:22 -p 8443:443 --privileged --name gls ex03

# git clone http://192.168.99.101:8080/root/gitlab_test_sam.git

# ssh -T -p 8080 -i ttester.pub git@192.168.99.101

# https://github.com/g0dReVeN/WTC-docker-1/blob/master/01_dockerfiles/ex03/Dockerfile

# docker build -t --build-arg ip=$(docker-machine ip Char) ex03 ./
