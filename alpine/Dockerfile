FROM alpine:latest

LABEL maintainer="snowdream <sn0wdr1am@icloud.com>"

ENV CPULIMIT_VERSION=0.2 \
    CPU_USAGE=90 \
    UTOPIA_PUBLIC_KEY=8619D1D54353041126B576AE44E79EE9E1C3243FC5F72FA11E8AE97241A09808  

RUN apk add --no-cache screen slang glibc libstdc++ build-base \
    &&  cd /root \
    &&  wget --no-check-certificate -c https://github.com/opsengine/cpulimit/archive/v${CPULIMIT_VERSION}.tar.gz \
    &&  tar zxvf v${CPULIMIT_VERSION}.tar.gz \
    &&  cd cpulimit-${CPULIMIT_VERSION} \
    &&  sed -i "/#include <sys\/sysctl.h>/d" src/cpulimit.c  \
    &&  make \
    &&  cp src/cpulimit /usr/bin/ \
    &&  cd /root \
    &&  wget --no-check-certificate -c https://update.u.is/downloads/linux/utopia-bot-latest.x86_64.tar.gz -O utopia-bot-latest.x86_64.tar.gz \
    &&  tar zxvf utopia-bot-latest.x86_64.tar.gz  \
    &&  rm -rf utopia-bot-latest.x86_64.tar.gz  \
    &&  wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub \
    &&  wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.30-r0/glibc-2.30-r0.apk  \
    &&  apk add glibc-2.30-r0.apk \
    &&  export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib:/lib:/lib64 \
    &&  ln -s  /lib/libz.so.1  /lib64/libz.so.1  \
    &&  cd /root \
    &&  rm -rf glibc-2.30-r0.apk \
    &&  rm -rf v${CPULIMIT_VERSION}.tar.gz \
    &&  rm -rf cpulimit-${CPULIMIT_VERSION} \
    &&  apk del build-base \
    &&  rm -rf /tmp/* /var/cache/apk/*

COPY docker-entrypoint.sh /usr/local/bin/
RUN ln -s usr/local/bin/docker-entrypoint.sh /entrypoint.sh # backwards compat

ENTRYPOINT ["docker-entrypoint.sh"]