# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.205.2/containers/ubuntu/.devcontainer/base.Dockerfile

# [Choice] Ubuntu version (use hirsuite or bionic on local arm64/Apple Silicon): hirsute, focal, bionic
# ARG VARIANT="bionic"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-bionic

# set mirror
RUN sed -i.bak -r 's!deb \S+!deb mirror://mirrors.ubuntu.com/mirrors.txt!' /etc/apt/sources.list \
# install software-properties-common(add-apt-repository)
    && apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends software-properties-common \
    && rm -rf /var/lib/apt/lists/* \
# for pypy3
    && add-apt-repository ppa:pypy/ppa -y \
# for nodejs
    && curl -sL https://deb.nodesource.com/setup_14.x | bash - \
# for .NET and powershell
    && wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && rm packages-microsoft-prod.deb \
    && apt-get update \
    && apt-get -y install --no-install-recommends \
# .NET
        dotnet-sdk-3.1 \
        dotnet-sdk-6.0 \
# Powershell
        powershell \
# Python3, PyPy3の3つの環境想定
        python3.8 \
        python3-pip \
        pypy3 \
# node
        nodejs \
# online-judge-tools用ライブラリ
        time \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
# update-alternatives
    && update-alternatives --install /usr/bin/python python /usr/bin/python3.8 30 \
    && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 30 \
    && update-alternatives --install /usr/bin/pypy pypy /usr/bin/pypy3 30 \
    && update-alternatives --install /usr/bin/node node /usr/bin/nodejs 30 \
# コンテスト補助アプリケーションをインストール
    && pip install online-judge-tools \
    && rm -rf ~/.cache/pip \
    && npm install -g atcoder-cli \
    && npm cache clean --force

SHELL ["pwsh","-command"]

# powershell セットアップ
RUN Set-PSRepository -Name PSGallery -InstallationPolicy Trusted \
    && Install-Module Pester,AngleParse,InvokeBuild,AtcoderFs.Pwsh  -Scope AllUsers -Confirm:$False -AcceptLicense  -Force -Verbose \
    && Install-Script -Name Invoke-Build.ArgumentCompleters  -Scope AllUsers -Confirm:$False -AcceptLicense  -Force -Verbose \
    && 'Import-Module AtcoderFs.Pwsh' | Add-Content $PROFILE.AllUsersAllHosts -Force