FROM node:16.11-buster

ENV DOCKER=YES

RUN apt-get update -y
RUN apt-get install -y tzdata
RUN apt-get install -y \
    make \
    cmake \
    git \
    libccid \
    libpcsclite-dev \
    libpcsclite1 \
    pcsc-tools \
    autoconf \
    pkg-config \
    g++
RUN apt-get clean


# Environment
ENV ******_PATH "/root/broadcast/S**C***"
ENV B25_PATH "/root/broadcast/libarib25"
ENV RECPT1_PATH "/root/broadcast/recpt1/recpt1"

COPY . /root/broadcast

# Build S**C***
RUN cd ${******_PATH} && make && cp -f libpcsclite.so.1.0.0 /lib/x86_64-linux-gnu/. && make clean

RUN ln -s /lib/x86_64-linux-gnu/libpcsclite.so.1.0.0 /lib/x86_64-linux-gnu/libpcsclite.so \
    && ln -s /lib/x86_64-linux-gnu/libpcsclite.so.1.0.0 /lib/x86_64-linux-gnu/libpcsclite.so.1

# Build libarib25
RUN cd ${B25_PATH} && cmake . && make && make install

# Build recpt1
RUN cd ${RECPT1_PATH} && ./autogen.sh && ./configure --enable-b25 && make && make install
RUN echo "/usr/local/lib" >> /etc/ld.so.conf.d/recpt1.conf

RUN ldconfig
# RUN modprobe pt3_drv


# Mirakurun
WORKDIR /root/broadcast/Mirakurun

RUN npm install && \
    npm run build && \
    npm install -g --unsafe-perm --production

CMD ["./docker/container-init.sh"]
EXPOSE 40772 9229
