FROM centos
MAINTAINER tomcat7-oracle-jdk1.7.0_65,troylc <liu_cheng@topsec.com.cn>

# 安装必备软件包
# Install prepare infrastructure
RUN yum -y update && \
	yum -y install wget && \
	yum -y install tar && \
	yum -y install vim-enhanced

#配置环境变量
ENV JAVA_HOME /opt/java
ENV CATALINA_HOME /opt/tomcat
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/bin:$CATALINA_HOME/scripts


# Install Oracle Java7
RUN wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" \
	http://download.oracle.com/otn-pub/java/jdk/7u65-b17/jdk-7u65-linux-x64.tar.gz && \
	tar -xvf jdk-7u65-linux-x64.tar.gz && \
	rm jdk*.tar.gz && \
	mv jdk* ${JAVA_HOME}

RUN ls ${JAVA_HOME}

# Install Tomcat
RUN wget http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.42/bin/apache-tomcat-7.0.42.tar.gz && \
	tar -xvf apache-tomcat-7.0.42.tar.gz && \
	rm apache-tomcat*.tar.gz && \
	mv apache-tomcat* ${CATALINA_HOME}

RUN ls ${CATALINA_HOME}
RUN chmod +x ${CATALINA_HOME}/bin/*



RUN java -version

# Create Tomcat admin user
ADD create_admin_user.sh $CATALINA_HOME/scripts/create_admin_user.sh
ADD tomcat.sh $CATALINA_HOME/scripts/tomcat.sh
RUN chmod +x $CATALINA_HOME/scripts/*.sh

# Create tomcat user
#RUN groupadd -r tomcat && \
#	useradd -g tomcat -d ${CATALINA_HOME} -s /sbin/nologin  -c "Tomcat user" tomcat && \
#	chown -R tomcat:tomcat ${CATALINA_HOME}

WORKDIR /opt/tomcat

EXPOSE 8080
EXPOSE 8009


USER tomcat
CMD ["tomcat.sh"]