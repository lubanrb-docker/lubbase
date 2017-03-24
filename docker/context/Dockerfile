# Environment: development.uber/lubbase
# Revision: 434766a7cfb81977f6d738ffbe2b5f9c

FROM centos:7
MAINTAINER Rubyist Chi "rubyist.chi@gmail.com"

ARG docker_engine_version
ARG docker_compose_version
ARG docker_gid
ARG docker_group
ARG luban_uid
ARG luban_user
ARG luban_root_path

# Create docker group with the given docker gid
RUN groupadd -g $docker_gid $docker_group

# Install required tools and libraries
RUN yum -y install \
        make \
        gcc \
        which \
        rsync \
        openssh-clients \
        zlib-devel \
        perl-devel \
        epel-release \
        nodejs \
        sqlite-devel \
        mysql-devel && \
    yum clean all && \
    \
    # Install docker engine
    yum -y install yum-utils && \ 
    yum-config-manager --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo && \
    yum makecache fast && yum -y install docker-ce-$docker_engine_version && \
    yum clean all && \
    \
    # Install docker compose
    curl -L "https://github.com/docker/compose/releases/download/$docker_compose_version/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
    chmod +x /usr/local/bin/docker-compose

# Create luban user with the given luban uid
# Adding luban user to the ftp group is to walk around an issue in mounting docker socket in OSX
RUN adduser -ms /bin/bash -u $luban_uid -G docker,ftp $luban_user && \
    echo "source /opt/luban/env/development.uber/lubbase/.luban_profile" >> /home/$luban_user/.bashrc && \
    mkdir -p $luban_root_path && \
    mkdir -p /opt/luban/projects && \
    mkdir -p /opt/luban/downloads && \
    mkdir -p /opt/luban/docker && \
    mkdir -p /opt/luban/packages && \
    mkdir -p /opt/luban/releases && \
    mkdir -p /opt/luban/env && \
    mkdir -p /opt/luban/tmp && \
    chown -R $luban_user:$luban_user $luban_root_path

# Add application archives
ADD uber-lubbase-packages-77cc3d71ce09.tar.xz \
    uber-lubbase-env.development-343a3587a9c7.tar.xz /

# RUN chown -R $luban_user:$luban_user $luban_root_path

# Populate image labels
LABEL luban.project="uber" \
      luban.application="uber" \
      luban.stage="development" \
      luban.packages.source_tag="77cc3d71ce09" \
      luban.env.development.source_tag="343a3587a9c7" \
      luban.build_tag="1.0.5" \
      luban.bulid_rev="852a1821c5f3" \
      luban.packages.curl.versions="7.53.1" \
      luban.packages.curl.current_version="7.53.1" \
      luban.packages.curl.before_install.openssl="1.1.0e" \
      luban.packages.git.versions="2.12.0" \
      luban.packages.git.current_version="2.12.0" \
      luban.packages.git.before_install.openssl="1.0.2k" \
      luban.packages.git.before_install.curl="7.53.1" \
      luban.packages.ruby.versions="2.4.0" \
      luban.packages.ruby.current_version="2.4.0" \
      luban.packages.ruby.before_install.openssl="1.1.0e" \
      luban.packages.ruby.after_install.rubygems="2.6.10" \
      luban.packages.ruby.after_install.bundler="1.14.4" \
      luban.docker_engine="$docker_engine_version" \
      luban.docker_compose="$docker_compose_version" \
      luban.docker_gid="$docker_gid" \
      luban.docker_group="$docker_group" \
      luban.luban_uid="$luban_uid" \
      luban.luban_user="$luban_user" \
      luban.luban_root_path="$luban_root_path"

USER $luban_user
WORKDIR /home/$luban_user

CMD ["/bin/bash"]
