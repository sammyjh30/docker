FROM debian

RUN apt-get update && \
    apt-get install wget -y bzip2 
RUN useradd -d /opt/teamspeak3-server -m teamspeak3-user
RUN wget http://dl.4players.de/ts/releases/3.8.0/teamspeak3-server_linux_amd64-3.8.0.tar.bz2
RUN tar -jxvf teamspeak3-server_linux_amd64-3.8.0.tar.bz2
RUN mkdir /teamspeak3-server
RUN mv teamspeak3-server_linux_amd64/* /teamspeak3-server
RUN chown teamspeak3-user:teamspeak3-user /teamspeak3-server -R
RUN rm -fr teamspeak3-server_linux_amd64-3.8.0.tar.bz2 teamspeak3-server_linux_amd64

ENV TS3SERVER_LICENSE=accept

EXPOSE 10011 30033 9987/udp

ENTRYPOINT sh ./teamspeak3-server/ts3server_minimal_runscript.sh start

#BUILD: docker build . -t ex01
#RUN: docker run -it --name ts -p 9987:9987/udp -p 30033:30033 -p10011:10011 --rm ex01


# root@e56abb84f854:/opt/teamspeak3-server# ./ts3server_minimal_runscript.sh
# https://terminal28.com/how-to-install-and-configure-teamspeak3-server-linux-debian-ubuntu/


# https://terminal28.com/how-to-install-and-configure-teamspeak3-server-linux-debian-ubuntu/
# https://linuxhint.com/dockerfile_expose_ports/ TCP = sending (default) UDP = Receiving
# https://forum.teamspeak.com/threads/46486-Teamspeak-3-Server-Ports For teamspeak port

# https://hub.docker.com/_/teamspeak



#  192.168.99.101