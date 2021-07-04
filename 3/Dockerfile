FROM mcr.microsoft.com/dotnet/core/sdk:3.1

MAINTAINER Jason A Turner<jason.turner@mycomplianec.ai>

RUN \
    apt-get update && apt-get install -y \
    bash groff libgdiplus gdebi-core
    # mkdir -p /aws && \
    # build-essential python3 docker zip python3-setuptools python3-dev python3-pip git sed jq  && \
    # pip3 install awscli --upgrade --user

COPY ./wkhtmltox_0.12.6-0.20180618.3.dev.e6d6f54.stretch_amd64.deb /

RUN gdebi --non-interactive /wkhtmltox_0.12.6-0.20180618.3.dev.e6d6f54.stretch_amd64.deb



WORKDIR /app
EXPOSE 3395

COPY build/ /app/

ENTRYPOINT [ "dotnet", "./CIQWatson.dll" ]
