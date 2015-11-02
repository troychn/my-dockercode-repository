#
# MAINTAINER        troylc <wwaa1983@163.com>
# DOCKER-VERSION    1.8.3
#
# Dockerizing CentOS7: Dockerfile for building CentOS images
#
FROM       centos
MAINTAINER helloWorld,troylc <wwaa1983@163.com>

ENV TZ "Asia/Shanghai"
ENV TERM xterm

RUN yum install -y curl tar openssh-server vim-enhanced passwd sudo hostname net-tools man && \
    yum install -y gcc gcc-c++ java-1.8.0-openjdk && \
    yum clean all

RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

# 添加测试用户admin，密码admin，并且将此用户添加到sudoers里
# RUN useradd admin
RUN echo "root:123456" | chpasswd
RUN echo "root   ALL=(ALL)       ALL" >> /etc/sudoers

# 下面这两句比较特殊，在centos6上必须要有，否则创建出来的容器sshd不能登录
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN mkdir /var/run/sshd

#下面是设置tomcat的环境变量以及工作目录
ENV CATALINA_HOME /usr/local/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
RUN mkdir -p "$CATALINA_HOME"
WORKDIR $CATALINA_HOME

# see https://www.apache.org/dist/tomcat/tomcat-8/KEYS
RUN gpg --keyserver pool.sks-keyservers.net --recv-keys \
	05AB33110949707C93A279E3D3EFE6B686867BA6 \
	07E48665A34DCAFAE522E5E6266191C37C037D42 \
	47309207D818FFD8DCD3F83F1931D684307A10A5 \
	541FBE7D8F78B25E055DDEE13C370389288584E7 \
	61B832AC2F1C5A90F0F9B00A1C506407564C17A3 \
	79F7026C690BAA50B92CD8B66A3AD3F4F22C4FED \
	9BA44C2621385CB966EBA586F72C284D731FABEE \
	A27677289986DB50844682F8ACB77FC2E86E29AC \
	A9C5DF4D22E99998D9875A5110C01C5A2F6059E7 \
	DCFD35E0BF8CA7344752DE8B6FB21E8933C60243 \
	F3A04C595DB5B6A5F1ECA43E3B7BBB100D811BBE \
	F7DA48BB64BCB84ECBA7EE6935CD23C10D498E23

ENV TOMCAT_MAJOR 8
ENV TOMCAT_VERSION 8.0.28
ENV TOMCAT_TGZ_URL https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz

RUN set -x \
	&& curl -fSL "$TOMCAT_TGZ_URL" -o tomcat.tar.gz \
	&& curl -fSL "$TOMCAT_TGZ_URL.asc" -o tomcat.tar.gz.asc \
	&& gpg --verify tomcat.tar.gz.asc \
	&& tar -xvf tomcat.tar.gz --strip-components=1 \
	&& rm bin/*.bat \
	&& rm tomcat.tar.gz*


EXPOSE 22
EXPOSE 8080

# CMD ["catalina.sh", "run"]
# 设置tomcat初始化运行，SSH终端服务作为后台运行
ENTRYPOINT catalina.sh run && /usr/sbin/sshd -D