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
    apt-get install -y iputils-ping && \
    apt-get install -y libenchant-dev && \
    apt-get install -y libgtk-3-0 && \
    apt-get install -y libgtksourceview-3.0-dev && \
    apt-get install -y libcroco-tools && \
    apt-get install -y libdbus-glib-1-2 && \
    apt-get install -y alien && \
    apt-get install -y xvfb && \
    apt-get install lsof && \
    apt-get install locales && \
    apt-get clean && \
    locale-gen en_US.UTF-8 && \
    echo "alias python3.3='/usr/bin/python3'" >> /root/.bashrc;
ENV LC_ALL en_US.UTF-8
RUN export LC_ALL
    
RUN wget https://repo.almalinux.org/almalinux/8.3-beta/AppStream/x86_64/os/Packages/libpng15-1.5.30-7.el8.x86_64.rpm && \
    wget http://ftp.altlinux.org/pub/distributions/ALTLinux/Sisyphus/x86_64/RPMS.classic/libunique3.0_0-3.0.2-alt1_14.x86_64.rpm && \
    wget http://ftp.altlinux.org/pub/distributions/ALTLinux/Sisyphus/x86_64/RPMS.classic/libnotify-0.7.9-alt1.x86_64.rpm && \
    alien -i libpng15-1.5.30-7.el8.x86_64.rpm && \
    alien -i libunique3.0_0-3.0.2-alt1_14.x86_64.rpm && \
    alien -i libnotify-0.7.9-alt1.x86_64.rpm && \
    ln -s /usr/lib64/libpng15.so.15 /lib/x86_64-linux-gnu/libpng15.so.15 && \
    ln -s /usr/lib64/libunique-3.0.so.0 /lib/x86_64-linux-gnu/libunique-3.0.so.0 && \
    ln -s /usr/lib64/libnotify.so.4 /lib/x86_64-linux-gnu/libnotify.so.4 && \
    rm libpng15-1.5.30-7.el8.x86_64.rpm libunique3.0_0-3.0.2-alt1_14.x86_64.rpm libnotify-0.7.9-alt1.x86_64.rpm
    
ENV DISPLAY :1
RUN export DISPLAY
    
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
