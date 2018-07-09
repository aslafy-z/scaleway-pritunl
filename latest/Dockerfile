## -*- docker-image-name: "scaleway/pritunl:latest" -*-
ARG SCW_ARCH

FROM scaleway/ubuntu:${SCW_ARCH}-xenial

MAINTAINER Scaleway <opensource@scaleway.com> (@scaleway)

# Prepare rootfs for image-builder
RUN /usr/local/sbin/builder-enter

# Copy files for apt configuration
COPY overlay/etc/apt /etc/apt

# Install dependencies
RUN  \
  apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 2930ADAE8CAF5059EE73BB4B58712A2291FA4AD5 \
  && apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A \
  && apt-get update \
  && apt-get -y install pritunl mongodb-org

# Enable services
RUN systemctl enable pritunl mongod

# Copy files
COPY overlay ./

# Create server specific doc in /root/ for first login
RUN systemctl enable create-doc

# Symlink doc readme to /root/
RUN ln -s /usr/share/doc/scaleway/pritunl/README /root/README

# Clean rootfs from image-builder
RUN /usr/local/sbin/builder-leave
