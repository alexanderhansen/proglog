FROM ubuntu:20.10

LABEL description="Dev container to develop go stuff with protobuf support."

# Config
ENV GO_VERSION=1.15.2
ENV PB_VERSION=3.13.0
ENV PATH "/usr/local/go/bin:/usr/local/protobuf/bin:${PATH}"
ENV GO111MODULE=on

# Create vscode user
ARG USERNAME=vscode
ARG USER_UID=1001
ARG USER_GID=$USER_UID

RUN set -o errexit -o nounset \
    && echo "Adding vscode user and group" \
    && groupadd --system --gid 1001 vscode \
    && useradd --system --gid vscode --uid 1001 --shell /bin/bash --create-home vscode \
    && mkdir /home/vscode/.vscode \
    && chown --recursive vscode:vscode /home/vscode \
    && usermod -a -G root vscode

# Install additional tools
RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y git curl unzip build-essential

# Golang
RUN curl -4 -L -k https://dl.google.com/go/go${GO_VERSION}.linux-amd64.tar.gz -o /tmp/go${GO_VERSION}.linux-amd64.tar.gz && \
    tar -xzvf /tmp/go${GO_VERSION}.linux-amd64.tar.gz -C /tmp && \
    mv /tmp/go /usr/local/go && \
    rm -rf /tmp/*

# Protobuf
RUN curl -4 -L -k https://github.com/protocolbuffers/protobuf/releases/download/v${PB_VERSION}/protoc-${PB_VERSION}-linux-x86_64.zip  -o /tmp/protobuf.zip && \
    unzip /tmp/protobuf.zip -d /usr/local/protobuf && \
    rm -rf /tmp/*

# Addtional go stuff
RUN go get -x github.com/gogo/protobuf/...@v1.3.1
