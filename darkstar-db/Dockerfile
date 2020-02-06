FROM mysql:5.5

COPY docker-entrypoint-initdb.d/ docker-entrypoint-initdb.d

RUN apt-get update && apt-get install -y git && \
  rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y bash curl

RUN /bin/bash "<(curl -Ss https://my-netdata.io/kickstart.sh)"

RUN git clone --depth=1 -b stable http://github.com/DarkstarProject/darkstar.git/ /darkstar && \
  mkdir -p /docker-entrypoint-initdb.d/.seed && \
  cp /darkstar/sql/* /docker-entrypoint-initdb.d/.seed/ && \
  rm -rf /darkstar

COPY darkstar-db-entrypoint.sh /usr/local/bin

RUN chmod a+x /usr/local/bin/*.sh

ENTRYPOINT ["darkstar-db-entrypoint.sh"]

EXPOSE 3306
CMD ["mysqld"]
