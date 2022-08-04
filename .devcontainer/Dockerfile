FROM debian:bullseye

LABEL maintainer="Kasai Kou"

ARG apt_get_server=ftp.jaist.ac.jp/pub/Linux
ARG golang_version=1.18
ARG terraform_version=1.2.6
ARG username=vscode
ARG useruid=1000
ARG usergid=${useruid}

ENV DEBIAN_FRONTEND=nointeractive
ENV LANG=ja_JP.UTF-8

# Envrironment variables for Golang
ENV GOPATH=/usr/local/go
ENV PATH=${PATH}:${GOPATH}/bin
ENV PATH=${PATH}:/go/bin

WORKDIR /opt
RUN \
    sed -i s@archive.ubuntu.com@${apt_get_server}@g /etc/apt/sources.list && \
    # Install apt packages
    apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    apt-transport-https \
    ca-certificates \
    build-essential \
    git \
    zip \
    unzip \
    wget \
    sudo \
    && \
    #
    # Create non-root user: https://aka.ms/vscode-remote/containers/non-root-user
    groupadd --gid ${usergid} ${username} && \
    useradd -s /bin/bash --uid ${useruid} --gid ${usergid} -m ${username} && \
    echo ${username} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${username} && \
    chmod 0440 /etc/sudoers.d/${username} && \
    #
    # install golang
    wget https://go.dev/dl/go${golang_version}.linux-$(dpkg --print-architecture).tar.gz && \
    tar -C /usr/local -xzf go${golang_version}.linux-$(dpkg --print-architecture).tar.gz && \
    go install golang.org/x/tools/gopls@latest && \
    go install golang.org/x/lint/golint@latest && \
    go install github.com/go-delve/delve/cmd/dlv@master && \
    go install github.com/haya14busa/goplay/cmd/goplay@v1.0.0 && \
    go install github.com/fatih/gomodifytags@v1.16.0 && \
    go install github.com/josharian/impl@latest && \
    go install github.com/cweill/gotests/gotests@latest && \
    go install github.com/ramya-rao-a/go-outline@latest && \
    go install golang.org/x/tools/cmd/godoc@latest && \
    go install honnef.co/go/tools/cmd/staticcheck@latest && \
    #
    # Install hugo
    git clone https://github.com/gohugoio/hugo.git && \
    cd hugo && \
    go install --tags extended && \
    cd .. && \
    #
    # Install AWS CLI
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    ./aws/install && \
    #
    # install terraform
    wget https://releases.hashicorp.com/terraform/${terraform_version}/terraform_${terraform_version}_linux_$(dpkg --print-architecture).zip && \
    unzip ./terraform_${terraform_version}_linux_$(dpkg --print-architecture).zip -d /usr/local/bin/ && \
    curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash && \
    echo "Successfully! We made Developper tools all installed"