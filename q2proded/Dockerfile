FROM ubuntu:24.04

ENV RELEASE="r3817"

WORKDIR /tmp/q2proded

# install required software
RUN set -xe; \
  apt-get update; \
  apt-get install -y build-essential zlib1g zlib1g-dev python3 ninja-build meson wget; \
# download and compile q2proded
  wget --no-check-certificate https://github.com/skullernet/q2pro/releases/download/nightly/q2pro-source.tar.gz; \
  tar zxf q2pro-source.tar.gz; \
  cd q2pro-${RELEASE}; \
  meson setup -Danticheat-server=true builddir; \
  meson compile -C builddir; \
  ninja -C builddir install; \
# remove all installed files
  rm -rf /tmp/q2proded; \
  apt-get purge -y build-essential zlib1g-dev python3 ninja-build meson; \
  apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
  apt-get autoremove; \
  rm -rf /var/lib/apt/lists/*

# default port for quake2 server
EXPOSE 27910

# change to ubuntu user to launch the server
USER ubuntu

# start a basic server
WORKDIR /home/ubuntu/.q2pro/
ENTRYPOINT ["/usr/local/bin/q2proded", "+set", "sys_console", "1"]
CMD ["+set", "game", "baseq2"]
