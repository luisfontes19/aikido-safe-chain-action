# Use Node.js 20 on Alpine Linux
FROM debian:stable-slim

# Set working directory
WORKDIR /app

RUN apt-get update && apt-get install -y curl git bash

RUN curl -sSf https://raw.githubusercontent.com/nektos/act/master/install.sh | bash
RUN mv bin/act /usr/local/bin/act
