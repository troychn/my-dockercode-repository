# ѡ��һ�����е�os������Ϊ����
FROM       centos
# ���������
MAINTAINER helloWorld,troylc <wwaa1983@163.com>

ENV TZ "Asia/Shanghai"
ENV TERM xterm

# ��װopenssh-server��sudo�����������ҽ�sshd��UsePAM�������ó�no
RUN yum install -y openssh-server sudo curl tar vim-enhanced passwd net-tools
RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

# ���Ӳ����û�admin������admin�����ҽ����û����ӵ�sudoers��
RUN useradd admin
RUN echo "admin:admin" | chpasswd
RUN echo "admin   ALL=(ALL)       ALL" >> /etc/sudoers
RUN echo "root:123456" | chpasswd

# ����������Ƚ����⣬��centos6�ϱ���Ҫ�У����򴴽�����������sshd���ܵ�¼
RUN ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key

# ����sshd�����ұ�¶22�˿�
RUN mkdir /var/run/sshd
EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]