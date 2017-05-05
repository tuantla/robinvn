FROM isuper/java-oracle:jdk_8
MAINTAINER robin <robin.github@gmail.com>
ENV TOMCAT_MAJOR_VERSION 7
ENV TOMCAT_MINOR_VERSION 7.0.34
ENV CATALINA_HOME /local/java/tomcat/${TOMCAT_MINOR_VERSION}
ENV JRE_HOME /usr/lib/jvm/java-8-oracle
ENV SOLR_OPTS " -Dsolr.solr.home=/var/lib/solr -Dsolr.data.dir=/var/lib/solr/data -Dsolr.slave.url=http://localhost:8080/solr -Dsolr.master.url=http://localhost:8080/solr "
ENV DB_OPTS " "
ENV MORE_OPTS " "
ENV CATALINA_OPTS ${SOLR_OPTS} ${DB_OPTS} ${MORE_OPTS}
# INSTALL TOMCAT

RUN  mkdir -p /local/java/tomcat;
RUN curl -s -o  /apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz  https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_MINOR_VERSION}/bin/apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz
#ADD https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_MINOR_VERSION}/bin/apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz /
#ADD apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz /
#RUN mv apache-tomcat-${TOMCAT_MINOR_VERSION} ${CATALINA_HOME}
RUN \    
    tar zxf apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz  && \
    rm apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz && \
    mv apache-tomcat-${TOMCAT_MINOR_VERSION} ${CATALINA_HOME}
ENV PATH $PATH:$CATALINA_HOME/bin:$JRE_HOME

RUN apt-get update && apt-get install -y telnet vim memcached nginx qpidd

# Remove unneeded apps

RUN \
   rm -rf ${CATALINA_HOME}/webapps/host-manager ; \   
   rm -rf ${CATALINA_HOME}/webapps/manager ; \ 
   rm -rf ${CATALINA_HOME}/webapps/examples  ;\
   rm -rf ${CATALINA_HOME}/webapps/docs/ ;\
   rm -rf ${CATALINA_HOME}/webapps/ROOT

RUN curl -s -o ${CATALINA_HOME}/lib/mail-1.4.3.jar  http://central.maven.org/maven2/javax/mail/mail/1.4.3/mail-1.4.3.jar && \
    curl -s -o ${CATALINA_HOME}/lib/mysql-connector-java-5.1.14.jar http://central.maven.org/maven2/mysql/mysql-connector-java/5.1.14/mysql-connector-java-5.1.14.jar && \
    curl -s -o ${CATALINA_HOME}/webapps/solr.war  http://central.maven.org/maven2/org/apache/solr/solr/3.4.0/solr-3.4.0.war
ADD log4j.xml ${CATALINA_HOME}/lib/
ADD solr /var/lib/solr
# tomcat conf
#ADD conf ${CATALINA_HOME}/conf
#ADD tomcat_wrapper.sh /opt/tomcat/bin/
#RUN ${CATALINA_HOME}/bin/catalina.sh run
ENTRYPOINT service memcached start && service qpidd start && service nginx start &&  ${CATALINA_HOME}/bin/catalina.sh run
EXPOSE 8080 11211 80 5672



