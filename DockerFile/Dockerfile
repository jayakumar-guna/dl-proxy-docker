FROM ubuntu

# Set Proxy
# ENV http_proxy http://192.168.100.100:3128/

RUN apt-get update && apt-get install gcc zlib1g-dev libreadline6-dev apt-utils make jq curl -y

RUN mkdir -p /tmp/downloads

ADD https://ftp.postgresql.org/pub/source/v9.4.0/postgresql-9.4.0.tar.gz /tmp/downloads

RUN cd /tmp/downloads && tar -zxf postgresql-9.4.0.tar.gz

RUN cd /tmp/downloads/postgresql-9.4.0 &&\
    make configure &&\
    ./configure &&\
    make &&\
    su &&\
    make install &&\
    adduser postgres --gecos '' --disabled-login &&\
    mkdir /usr/local/pgsql/data &&\
    chown postgres /usr/local/pgsql/data

RUN cd /tmp/downloads/postgresql-9.4.0/contrib/pgcrypto/ &&\
    make &&\
    make install &&\
    cd /tmp/downloads/postgresql-9.4.0/contrib/citext/ &&\
    make &&\
    make install

USER postgres

# use below command only if it is necessary, it is similar to "cd" linux command
WORKDIR /tmp/downloads/postgresql-9.4.0
RUN /usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data

USER root

WORKDIR /

# Install OpenJDK-8
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get install -y ant && \
    apt-get clean;

# Setup JAVA_HOME
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME

# Install needed dependencies 
RUN apt-get update && \
    apt-get install -y git && \
    apt-get install -y vim && \
    apt-get install -y wget && \
    apt-get install -y python3-pip && \
    apt-get install -y mercurial && \
    apt-get install -y zip && \
    apt-get install lsof && \
    apt-get clean && \
    echo "alias python3.3='/usr/bin/python3'" >> /root/.bashrc;
    
# Install newman
RUN apt-get install aptitude -y && \
    aptitude install npm -y && \
    npm install newman -g;
    
# ENV http_proxy=

# Add ANT related dependencies
# ADD http://cm-server/thirdparty_downloads/ant-contrib/ant-contrib-1.0b1.jar /usr/share/ant/lib/
# ADD http://cm-server/thirdparty_downloads/ant/apache-ant-1.9.6.zip /usr/lib/
# ENV ANT_HOME=/usr/lib/apache-ant-1.9.6.zip
# ENV JAVA_TOOL_OPTIONS=-Dfile.encoding=UTF-8
