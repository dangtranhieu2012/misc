FROM ubuntu:18.04

RUN apt-get update \
    && apt-get install -y gawk wget git-core diffstat unzip texinfo \
       gcc-multilib build-essential chrpath socat cpio python python3 \
       python3-pip python3-pexpect xz-utils debianutils iputils-ping \
       libegl1-mesa libsdl1.2-dev xterm g++-multilib zip apt-transport-https \
       ca-certificates curl gnupg lsb-release tar locales sudo bc git-lfs

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
    | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

RUN curl https://storage.googleapis.com/git-repo-downloads/repo > /usr/bin/repo \
    && chmod +x /usr/bin/repo

RUN echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

ARG host_docker_gid
RUN apt-get update \
    && apt-get install -y docker-ce docker-ce-cli containerd.io \
    && groupmod -g $host_docker_gid docker

RUN ln -sf /bin/bash /bin/sh

RUN locale-gen en_US.UTF-8 && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

ENV USERNAME=builder

ARG host_uid
ARG host_gid
RUN groupadd -g $host_gid $USERNAME \
    && useradd -g $host_gid -m -s /bin/bash -u $host_uid $USERNAME \
    && usermod -aG docker $USERNAME \
    && echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

USER $USERNAME

ARG git_username
ARG git_email

RUN git config --global user.name "$git_username" \
    && git config --global user.email "$git_email"
