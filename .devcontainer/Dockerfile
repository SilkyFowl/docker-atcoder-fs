# 参考:https://qiita.com/hinamimi/items/b3dd159f956628cebdbb

# [Choice] Debian / Ubuntu version (use Debian 11/9, Ubuntu 18.04/21.04 on local arm64/Apple Silicon): debian-11, debian-10, debian-9, ubuntu-21.04, ubuntu-20.04, ubuntu-18.04
ARG VARIANT="bullseye"
FROM mcr.microsoft.com/vscode/devcontainers/cpp:0-${VARIANT}

# [Optional] Uncomment this section to install additional packages.
# RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

ENV TZ=Asia/Tokyo DOTNET_CLI_TELEMETRY_OPTOUT=1

# set mirror
RUN sed -i.bak -r 's!deb \S+!deb mirror://mirrors.ubuntu.com/mirrors.txt!' /etc/apt/sources.list \
# install software-properties-common(add-apt-repository)
    && apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends software-properties-common \
    && rm -rf /var/lib/apt/lists/* \
# for pypy3
    && add-apt-repository ppa:pypy/ppa \
# for nodejs
    && curl -sL https://deb.nodesource.com/setup_14.x | bash - \
# for .NET and powershell
    && wget https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && rm packages-microsoft-prod.deb \
    && apt-get update && export DEBIAN_FRONTEND=noninteractive \
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
# update-alternatives
    && update-alternatives --install /usr/bin/python python /usr/bin/python3.8 30 \
    && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 30 \
    && update-alternatives --install /usr/bin/pypy pypy /usr/bin/pypy3 30 \
    && update-alternatives --install /usr/bin/node node /usr/bin/nodejs 30 \
# コンテスト補助アプリケーションをインストール
    && pip install online-judge-tools \
    && npm install -g atcoder-cli

SHELL ["pwsh","-command"]

# powershell セットアップ
RUN Set-PSRepository -Name PSGallery -InstallationPolicy Trusted \
    && Install-Module Pester,AngleParse,InvokeBuild,AtcoderFs.Pwsh  -Scope AllUsers -Confirm:$False -AcceptLicense  -Force -Verbose \
    && Install-Script -Name Invoke-Build.ArgumentCompleters  -Scope AllUsers -Confirm:$False -AcceptLicense  -Force -Verbose \
    && 'Import-Module AtcoderFs.Pwsh' | Add-Content $PROFILE.AllUsersAllHosts -Force \
# .NET tool セットアップ
    && dotnet tool install Paket --prerelease --tool-path /usr/local/share/dotnet-tools \
    && dotnet tool install fantomas-tool --prerelease --tool-path /usr/local/share/dotnet-tools \
    && dotnet tool install fake-cli --prerelease --tool-path /usr/local/share/dotnet-tools

ENV PATH="${PATH}:/usr/local/share/dotnet-tools"