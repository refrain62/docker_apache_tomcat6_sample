FROM centos:7
RUN yum -y update && yum clean all

# Apacheのインストール
RUN yum -y install httpd httpd-devel gcc* make && yum clean all

# yum httpd/wget/java のインストール
RUN yum -y install wget java-1.8.0-openjdk

# apache 設定
#RUN mkdir -p /var/www/html/
#RUN mkdir /var/www/cgi-bin/
#RUN chown -R apache.apache /var/www/
RUN chmod 777 /var/www/

RUN mkdir -p /var/www/vhosts/web.local
RUN mkdir /var/www/vhosts/web.local/html
RUN mkdir /var/www/vhosts/web.local/log
RUN chown apache.apache /var/www/vhosts/web.local
RUN chmod 777 /var/www/vhosts/web.local

COPY ./httpd/conf/web.conf /etc/httpd/conf.d/
#COPY run-httpd.sh /run-httpd.sh
#RUN chmod -v +x /run-httpd.sh
#CMD ["/run-httpd.sh"]

#RUN echo "ServerName $SERVERNAME" >> /usr/local/apache2/conf/httpd.conf
RUN echo "ServerName web.local" >> /etc/httpd/conf/httpd.conf

# tomcat 設定
RUN wget http://archive.apache.org/dist/tomcat/tomcat-6/v6.0.53/bin/apache-tomcat-6.0.53.tar.gz
RUN tar zxvf apache-tomcat-6.0.53.tar.gz
RUN mv apache-tomcat-6.0.53 /usr/local/tomcat6
RUN cp /usr/local/tomcat6/bin/catalina.sh /etc/init.d/tomcat6
RUN chmod 775 /etc/init.d/tomcat6
ENV CATALINA_HOME=/usr/local/tomcat6
ENV CATALINA_BASE=/usr/local/tomcat6
ENV CATALINA_TMPDIR=/usr/local/tomcat6/temp
ENV JRE_HOME=/usr
ENV CLASSPATH=/usr/local/tomcat/bin/bootstrap.jar

COPY ./tomcat/conf/server.xml /usr/local/tomcat6/conf/server.xml
COPY ./tomcat/conf/tomcat-users.xml /usr/local/tomcat6/conf/tomcat-users.xml

# いらないTomcatの内容を削除
#RUN rm -rf /usr/local/tomcat/webapps/docs
#RUN rm -rf /usr/local/tomcat/webapps/examples
#RUN rm -rf /usr/local/tomcat/webapps/host-manager
#RUN rm -rf /usr/local/tomcat/webapps/manager
RUN rm -rf /usr/local/tomcat/webapps/ROOT

# サンプルのプロジェクトをデプロイ
COPY ./tomcat/webapps/sample.war /usr/local/tomcat6/webapps/sample.war

#httpd,tomcat6の起動スクリプトの作成
RUN echo -e "/etc/init.d/tomcat6 start\n/usr/sbin/httpd -DFOREGROUND\n/bin/bash" > /startService.sh

#httpd,tomcat6の起動スクリプトの権限設定
RUN chmod o+x /startService.sh

#公開ポート(apache:80,tomcat(http):8080で空けてる)
EXPOSE 80 8080

#httpd,tomcat6の起動スクリプトの実行
CMD /startService.sh
