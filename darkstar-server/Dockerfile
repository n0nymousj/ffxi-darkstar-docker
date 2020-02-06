FROM ubuntu

ENV DS_BRANCH=stable

RUN apt-get update

#RUN  apt-get install software-properties-common

#RUN add-apt-repository -y ppa:ubuntu-toolchain-r/test

#RUN apt-get install -y build-essential

RUN apt-get install -y git libmysqlclient-dev libluajit-5.1-dev libzmq3-dev autoconf pkg-config python-pip libssl-dev

RUN apt-get install -y nano curl

RUN apt-get update && \
  cd /usr/bin && \
  pip install supervisor && \
  pip install supervisor-stdout

RUN curl -Ss https://my-netdata.io/kickstart.sh --dont-wait

RUN git clone --depth=1 -b ${DS_BRANCH} http://github.com/DarkstarProject/darkstar.git/ /darkstar && \
  cd /darkstar && \
  sh autogen.sh && \
  ./configure --enable-debug=gdb && \
  make -j6 && \
  apt-get autoremove -y build-essential git autoconf pkg-config software-properties-common && \
  rm -rf /var/lib/apt/lists/* && \
  rm -rf /darkstar/src && \
  rm -rf /darkstar/sql

COPY ./etc/* etc/
COPY ./docker-entrypoint.sh /usr/local/bin/

# add darkstar user and fix permissions
RUN groupadd -r darkstar && \
  useradd -g darkstar -ms /bin/bash darkstar && \
  chown -R darkstar:darkstar /darkstar && \
  chmod a+x /usr/local/bin/docker-entrypoint.sh

USER darkstar
EXPOSE 54230 54230/udp 54231 54001 54002
WORKDIR /darkstar

CMD ["docker-entrypoint.sh"]
