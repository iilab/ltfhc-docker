FROM debian:wheezy

MAINTAINER Jun Matsushita jun@iilab.org

ADD sources.list /etc/apt/sources.list

RUN apt-get update && apt-get install -y build-essential wget curl

# erlang and couch deps
RUN apt-get install -y erlang-base-hipe \
  erlang-xmerl \
  erlang-inets \
  erlang-manpages \
  erlang-dev erlang-nox \
  erlang-eunit \
  libmozjs185-dev \
  libicu-dev \
  libcurl4-gnutls-dev \
  libtool

# Get the source
RUN cd /opt && \
 wget http://apache.openmirror.de/couchdb/source/1.6.0/apache-couchdb-1.6.0.tar.gz && \
 tar xzf /opt/apache-couchdb-1.6.0.tar.gz

# build couchdb
RUN cd /opt/apache-couchdb-* && ./configure && make && make install

# install github.com/visionmedia/mon v1.2.3
RUN (mkdir /tmp/mon && cd /tmp/mon && curl -L# https://github.com/visionmedia/mon/archive/1.2.3.tar.gz | tar zx --strip 1 && make install)

# cleanup
# RUN apt-get remove -y build-essential wget curl && \
#  apt-get autoremove -y && apt-get clean -y && \
#  rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /opt/apache-couchdb-*

ADD ./opt /opt

# Configuration
RUN sed -e 's/^bind_address = .*$/bind_address = 0.0.0.0/' -i /usr/local/etc/couchdb/default.ini
RUN /opt/couchdb-config

RUN apt-get update && apt-get install -y curl libev4 libssl-dev libev-dev
RUN (mkdir /tmp/stud && cd /tmp/stud && curl -L# https://github.com/bumptech/stud/archive/master.tar.gz | tar zx --strip 1 && make && make install && rm -rf /tmp/stud)

# cleanup
RUN apt-get remove -y curl && \
 apt-get autoremove -y && apt-get clean -y && \
 rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD stud.conf /usr/local/etc/stud/
ADD generate-pem /root/
ADD start /opt/
ADD stud-config /opt/

RUN ./root/generate-pem
RUN ./opt/stud-config

# Define mountable directories.
VOLUME ["/usr/local/var/log/couchdb", "/usr/local/var/lib/couchdb", "/usr/local/etc/couchdb"]

EXPOSE 493
CMD ["/opt/start"]