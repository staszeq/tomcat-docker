FROM centos
MAINTAINER staszeq <akoniuszy@gmail.com>

ENV TOMCAT_MAJOR 7
ENV TOMCAT_VERSION 7.0.70
ENV TOMCAT_URL https://www.apache.org/dist/tomcat/tomcat-$TOMCAT_MAJOR/v$TOMCAT_VERSION/bin/apache-tomcat-$TOMCAT_VERSION.tar.gz

ENV CATALINA_HOME /opt/tomcat
ENV PATH $CATALINA_HOME/bin:$PATH
WORKDIR $CATALINA_HOME

RUN yum -y install java \
    && yum clean all \
    && groupadd -r tomcat -g 1000 \
    && useradd -u 1000 -r -g tomcat -m -d /opt/tomcat -s /sbin/nologin -c "Tomcat user" tomcat \
    && chmod 755 /opt/tomcat

RUN cd $CATALINA_HOME \
    && curl -o tomcat.tar.gz $TOMCAT_URL \
    && tar -xvf tomcat.tar.gz -C /opt/tomcat --strip-components=1 \
    && rm tomcat.tar.gz \
    && rm -rf $CATALINA_HOME/webapps/ROOT \
    && rm -rf $CATALINA_HOME/webapps/docs \
    && rm -rf $CATALINA_HOME/webapps/examples \
    && rm -rf $CATALINA_HOME/webapps/host-manager

COPY conf  $CATALINA_HOME/conf
ADD version.war $CATALINA_HOME/webapps/
RUN chown -R tomcat:tomcat /opt/tomcat
USER tomcat

EXPOSE 8080
CMD ["catalina.sh", "run"]
