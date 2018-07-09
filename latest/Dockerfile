## -*- docker-image-name: "scaleway/pritunl:latest" -*-
ARG SCW_ARCH

FROM scaleway/ubuntu:${SCW_ARCH}-xenial

MAINTAINER Scaleway <opensource@scaleway.com> (@scaleway)

# Prepare rootfs for image-builder
RUN /usr/local/sbin/builder-enter

# Copy source apt files
COPY ./overlay/etc/apt/ /etc/apt

# Configure apt
ARG DEBIAN_FRONTEND=noninteractive

# Install dependencies
RUN  \
  apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5 \
  && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A \
  && apt-get update \
  && apt-get -y install pritunl mongodb-org

# Fix pritunl package inconsistency
RUN chmod 644 /etc/systemd/system/pritunl.service

# Tweek system limits
# from https://docs.pritunl.com/docs/configuration-5#section-increase-open-file-limit
RUN \
  echo "* hard nofile 64000" >> /etc/security/limits.conf \
  && echo "* soft nofile 64000" >> /etc/security/limits.conf \
  && echo "root hard nofile 64000" >> /etc/security/limits.conf \
  && echo "root soft nofile 64000" >> /etc/security/limits.conf

# Enable autostart of services
RUN systemctl enable pritunl mongod

# Copy files
COPY ./overlay/ /

# Create server specific doc in /root/ for first login
# RUN systemctl enable create-doc

# Symlink doc readme to /root/
RUN ln -s /usr/share/doc/scaleway/pritunl/README /root/README

# Clean rootfs from image-builder
RUN /usr/local/sbin/builder-leave
