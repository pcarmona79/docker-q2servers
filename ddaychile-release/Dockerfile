FROM q2admin-kmod:1.17.51-kmod

ENV RELEASE="5.062"

USER root
WORKDIR /tmp/

# compile and copy .so files to libdir directory where it will be found by q2admin
RUN set -xe; \
  apt-get update; \
  apt-get install -y gcc libc6-dev make; \
  export ARCH=$(uname -m); \
  export Q2PROARCH=$(if [ \"aarch64\" = \"$ARCH\" ]; then echo "arm64"; else echo $ARCH; fi); \
  wget --no-check-certificate -O release-dll-src.tar.gz https://github.com/ddaychile/release-dll/archive/refs/tags/${RELEASE}.tar.gz; \
  tar -zxf release-dll-src.tar.gz; \
  cd release-dll-${RELEASE}/src; \
#  git config --global --add safe.directory /tmp/dday-old-vibes; \
  make; \
#  git config --global --unset safe.directory /tmp/dday-old-vibes; \
# copy compiled .so files
  mkdir -p /usr/local/lib/${ARCH}-linux-gnu/q2pro/dday/; \
  cp game${ARCH}.real.so /usr/local/lib/${ARCH}-linux-gnu/q2pro/dday/game${ARCH}.real.so; \
# copy q2admin from baseq2
  cp /usr/local/lib/${ARCH}-linux-gnu/q2pro/baseq2/game${Q2PROARCH}.so /usr/local/lib/${ARCH}-linux-gnu/q2pro/dday/; \
# remove compile files
  rm -rf /tmp/release-dll-src.tar.gz /tmp/release-dll-${RELEASE}; \
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
CMD ["+set", "game", "dday", "+exec", "server.cfg"]
