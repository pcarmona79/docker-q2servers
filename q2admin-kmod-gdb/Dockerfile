FROM q2proded:r3832

ENV RELEASE="1.17.51-kmod-gdb"

USER root
WORKDIR /tmp/

# download and compile
RUN set -xe; \
  apt-get update; \
  apt-get install -y gcc libc6-dev make; \
  export ARCH=$(uname -m); \
  export Q2PROARCH=$(if [ \"aarch64\" = \"$ARCH\" ]; then echo "arm64"; else echo $ARCH; fi); \
# download source form github
  wget --no-check-certificate -O q2admin-kmod-src.tar.gz https://github.com/pcarmona79/q2admin-kmod/archive/refs/tags/${RELEASE}.tar.gz; \
  tar -zxf q2admin-kmod-src.tar.gz; \
  cd q2admin-kmod-${RELEASE}; \
  make build_debug; \
# rename game lib so we can place the q2admin lib instead
  mv /usr/local/lib/${ARCH}-linux-gnu/q2pro/baseq2/game${Q2PROARCH}.so /usr/local/lib/${ARCH}-linux-gnu/q2pro/baseq2/game${ARCH}.real.so; \
  cp game${ARCH}.so /usr/local/lib/${ARCH}-linux-gnu/q2pro/baseq2/game${Q2PROARCH}.so; \
# remove compile files
  rm -rf /tmp/q2admin-kmod-src.tar.gz /tmp/q2admin-kmod-${RELEASE}; \
  apt-get purge -y gcc libc6-dev make; \
  apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
  apt-get autoremove; \
  rm -rf /var/lib/apt/lists/*

# default port for quake2 server
EXPOSE 27910/udp

# change to ubuntu user to launch the server
USER ubuntu

# start the server
WORKDIR /home/ubuntu/.q2pro/
CMD ["+set", "game", "baseq2", "+exec", "server.cfg"]
