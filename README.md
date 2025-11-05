# testt



FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    vim \
    ca-certificates \
    curl \
    gnupg \
    apt-transport-https \
    openssh-server \
    postfix \
    python3-minimal \
    && rm -rf /var/lib/apt/lists/*

RUN curl -fsSL https://raw.githubusercontent.com/gdraheim/docker-systemctl-replacement/master/files/docker/systemctl.py -o /usr/local/bin/systemctl && \
    chmod +x /usr/local/bin/systemctl

RUN curl -fsSL https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey | gpg --dearmor -o /usr/share/keyrings/gitlab-ee-archive-keyring.gpg

RUN echo "deb [signed-by=/usr/share/keyrings/gitlab-ee-archive-keyring.gpg] https://packages.gitlab.com/gitlab/gitlab-ee/ubuntu/ jammy main" > /etc/apt/sources.list.d/gitlab_gitlab-ee.list

RUN apt-get update && apt-get install -y gitlab-ee && rm -rf /var/lib/apt/lists/*

CMD ["bash"]
