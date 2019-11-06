# ONBUILD ADD . /app/src
# ONBUILD RUN /usr/local/bin/python-build --dir /app/src

ONBUILD FROM ruby:2.3

ONBUILD RUN apt-get update && \
    apt-get install -y --no-install-recommends && \
    mkdir /opt/

ONBUILD WORKDIR /opt/app

# https://hub.docker.com/_/rails/