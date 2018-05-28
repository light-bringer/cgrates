FROM debian:jessie
MAINTAINER Radu Fericean(rif@cgrates.org), Debapriya Das(yodebu@gmail.com)
RUN apt-get -y update
RUN printf "deb http://httpredir.debian.org/debian jessie-backports main non-free\ndeb-src http://httpredir.debian.org/debian jessie-backports main non-free" > /etc/apt/sources.list.d/backports.list
RUN apt-get update && apt-get -y install gnupg2
RUN apt-get install -y apt-transport-https
# set mysql password
RUN echo 'mysql-server mysql-server/root_password password CGRateS.org' | debconf-set-selections && echo 'mysql-server mysql-server/root_password_again password CGRateS.org' | debconf-set-selections
    
# add freeswitch gpg key
RUN for server in $(shuf -e ha.pool.sks-keyservers.net \
                            hkp://p80.pool.sks-keyservers.net:80 \
                            keyserver.ubuntu.com \
                            hkp://keyserver.ubuntu.com:80 \
                            pgp.mit.edu) ; do \
        gpg --keyserver "$server" --recv-keys D76EDC7725E010CF && break || : ; \
    done \
    && gpg -a --export D76EDC7725E010CF | apt-key add -

# add freeswitch apt repo
RUN echo 'deb http://files.freeswitch.org/repo/deb/debian/ jessie main' > /etc/apt/sources.list.d/freeswitch.list

# install dependencies
RUN apt-get update && apt-get -y install freeswitch redis-server mysql-server python-pycurl libfreeswitch1 python-mysqldb postgresql postgresql-client sudo wget git freeswitch-meta-vanilla

# # add cgrates apt-key
# RUN wget -qO- http://apt.itsyscom.com/conf/cgrates.gpg.key | apt-key add -
# 
# # add cgrates repo
# RUN cd /etc/apt/sources.list.d/; wget -q http://apt.itsyscom.com/conf/cgrates.apt.list
# 
# # install cgrates
# RUN apt-get update && apt-get -y install cgrates

# install cgrates from debian pkg
RUN wget http://www.cgrates.org/tmp_pkg/cgrates_0.9.1~rc8_amd64.deb \
    && dpkg -i cgrates_0.9.1~rc8_amd64.deb

# cleanup
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD start.sh /root/start.sh
# set start command
CMD /root/start.sh

