FROM tomcat:9-jdk11

ENV DOMIBUS_VERSION 4.2.1
ENV DOMIBUS_URL https://ec.europa.eu/cefdigital/artifact/repository/eDelivery/eu/domibus/domibus-distribution/$DOMIBUS_VERSION/domibus-distribution-$DOMIBUS_VERSION-tomcat-full.zip
ENV TEMP_DIR /usr/local/domibus-temp
ENV DOMIBUS_DIR /usr/local/tomcat
ENV CATALINA_HOME $DOMIBUS_DIR/domibus
ENV MYSQL_CONNECTOR_VERSION 8.0.23
ENV DOCKERIZE_VERSION v0.6.1

RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install -y unzip
RUN apt-get install -y curl

RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN wget $DOMIBUS_URL && unzip -d $DOMIBUS_DIR domibus-distribution-$DOMIBUS_VERSION-tomcat-full.zip

RUN wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-$MYSQL_CONNECTOR_VERSION.zip && unzip -d $TEMP_DIR mysql-connector-java-$MYSQL_CONNECTOR_VERSION.zip
RUN cp $TEMP_DIR/mysql-connector-java-$MYSQL_CONNECTOR_VERSION/mysql-connector-java-$MYSQL_CONNECTOR_VERSION.jar $CATALINA_HOME/lib

RUN rm $CATALINA_HOME/bin/setenv.sh
RUN touch $CATALINA_HOME/bin/setenv.sh

RUN echo '#!/bin/bash' >> $CATALINA_HOME/bin/setenv.sh
RUN echo "export CATALINA_HOME=$CATALINA_HOME" >> $CATALINA_HOME/bin/setenv.sh
RUN echo "export CATALINA_TMPDIR=$CATALINA_HOME/temp" >> $CATALINA_HOME/bin/setenv.sh
RUN echo 'export JAVA_OPTS="$JAVA_OPTS -Xms128m -Xmx1024m"' >> $CATALINA_HOME/bin/setenv.sh
RUN echo 'export JAVA_OPTS="$JAVA_OPTS -Ddomibus.config.location=$CATALINA_HOME/conf/domibus"' >> $CATALINA_HOME/bin/setenv.sh

RUN chmod 777 $CATALINA_HOME/bin/*.sh

EXPOSE 8080

WORKDIR $DOMIBUS_DIR
ENTRYPOINT ["./bin/startup.sh"]