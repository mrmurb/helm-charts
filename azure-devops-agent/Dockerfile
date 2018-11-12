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

CMD ["./start.sh"]
