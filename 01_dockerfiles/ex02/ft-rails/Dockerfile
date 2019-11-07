# ONBUILD ADD . /app/src
# ONBUILD RUN /usr/local/bin/python-build --dir /app/src

FROM ruby:2.3

RUN apt-get update && \
    apt-get install -y --no-install-recommends && \
    mkdir /opt/app

ONBUILD WORKDIR /opt/app
ONBUILD ADD . /opt/app

# https://hub.docker.com/_/rails/