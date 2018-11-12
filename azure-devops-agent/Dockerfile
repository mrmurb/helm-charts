FROM debian:jessie

ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

# Miscellaneous
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        apt-utils curl git jq libicu52 libunwind8 netcat ca-certificates \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /etc/apt/sources.list.d/*

WORKDIR /vsts
COPY ./scripts/start.sh .
RUN chmod +x start.sh

# .NET Core SDK
ENV DOTNET_SKIP_FIRST_TIME_EXPERIENCE true
ADD https://dot.net/v1/dotnet-install.sh .
RUN chmod +x dotnet-install.sh
RUN ./dotnet-install.sh --channel Current --install-dir /opt/dotnet
ENV dotnet /opt/dotnet/dotnet

# Node.js LTS
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /etc/apt/sources.list.d/*

# Yarn
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends yarn \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /etc/apt/sources.list.d/*

# Kubectl
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
    && chmod +x ./kubectl \
    && mv ./kubectl /usr/local/bin/kubectl

# Helm
RUN curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash

# Docker
ENV DOCKER_CHANNEL stable
ENV DOCKER_VERSION 17.06.0-ce
RUN set -ex \
    && curl -fL "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/`uname -m`/docker-${DOCKER_VERSION}.tgz" -o docker.tgz \
    && tar --extract --file docker.tgz --strip-components 1 --directory /usr/local/bin \
    && rm docker.tgz \
    && docker -v

CMD ["./start.sh"]
