# Environment: development.uber/lubbase
# Revision: e39ea6b779a1dda1a60a7ddeab8a7111

FROM centos:7
MAINTAINER Chi Man Lei "chi.lei@laxino.com"

RUN adduser -ms /bin/bash luban && \
    mkdir /opt/luban && \
    chown -R luban:luban /opt/luban

# Install required tools and libs
RUN yum -y install make gcc which rsync && yum clean all

# Install required libs
RUN yum -y install zlib-devel perl-devel && yum clean all

# Install docker
RUN yum -y install yum-utils && \ 
    yum-config-manager --add-repo \
    https://docs.docker.com/engine/installation/linux/repo_files/centos/docker.repo && \
    yum makecache fast && yum -y install docker-engine && yum clean all && \
    curl -L "https://github.com/docker/compose/releases/download/1.10.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

RUN usermod -aG docker luban && \
    usermod -aG ftp luban # To walkaround the issue of mounting docker socket in OSX

RUN mkdir /opt/luban/projects && \
    mkdir /opt/luban/downloads && \
    mkdir /opt/luban/docker && \
    mkdir /opt/luban/tmp && \
    mkdir /opt/luban/packages && \
    mkdir /opt/luban/releases && \
    mkdir /opt/luban/env && \
    chown -R luban:luban /opt/luban

VOLUME /opt/luban/projects \
       /opt/luban/downloads \
       /opt/luban/docker \
       /opt/luban/packages \
       /opt/luban/releases \
       /opt/luban/env

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo "source /opt/luban/env/development.uber/lubbase/.luban_profile" >> /home/luban/.bashrc

ADD uber-lubbase-packages-2a4d23455e0d.tar.xz /
ADD uber-lubbase-development-343a3587a9c7.tar.xz /

LABEL luban.project="uber" \
      luban.application="uber" \
      luban.stage="development" \
      luban.packages_tag="2a4d23455e0d" \
      luban.development_tag="343a3587a9c7" \
      luban.build_tag="1.0.0" \
      luban.bulid_rev="d41d8cd98f00" \
      luban.luban_user="luban" \
      luban.luban_root_path="/opt/luban"

USER luban
WORKDIR /home/luban

CMD ["/bin/bash"]
