FROM ubuntu:focal

# necessary envs
ENV container docker
ARG LC_ALL=C
ARG DEBIAN_FRONTEND=noninteractive

# change source.list
COPY sources.list /etc/apt/sources.list

# install packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    systemd openssh-server ubuntu-minimal build-essential \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# setup systemd services
RUN rm -f /lib/systemd/system/sysinit.target.wants/*.mount \
    && ln -sf /lib/systemd/system/multi-user.target /etc/systemd/system/default.target \
    && rm -f /etc/systemd/system/multi-user.target.wants/networkd-dispatcher.service \
    && sed -i 's/#PermitRootLogin.*/PermitRootLogin\ yes/' /etc/ssh/sshd_config \
    && cp /lib/systemd/system/ssh.service /etc/systemd/system/ssh.service \
    && sed -i '/^After=.*/d' /etc/systemd/system/ssh.service \
    && ln -sf /etc/systemd/system/ssh.service /etc/systemd/system/multi-user.target.wants/

# set password
RUN echo 'root:root' | chpasswd

STOPSIGNAL SIGRTMIN+3

EXPOSE 22
WORKDIR /root
VOLUME ["/sys/fs/cgroup", "/tmp", "/run", "/run/lock"]

CMD ["/lib/systemd/systemd"]
