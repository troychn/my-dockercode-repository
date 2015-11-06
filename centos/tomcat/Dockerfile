FROM toptomcat
MAINTAINER Topsec <tian@topsec.com.cn>


ADD create_tomcat_admin_user.sh /create_tomcat_admin_user.sh
ADD run.sh /run.sh
RUN chmod +x /*.sh

EXPOSE 22
EXPOSE 8080
CMD ["/run.sh"]