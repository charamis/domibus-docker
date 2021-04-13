FROM tomcat:9-jdk11

ENV DOMIBUS_VERSION 4.2.1

ENV TOMCAT_VERSION 9
ENV DOCKERIZE_VERSION v0.6.1

ENV MYSQL_CONNECTOR mysql-connector-java-8.0.23
ENV DOMIBUS_DIST /usr/local/domibusDist
ENV TOMCAT_FULL_DISTRIBUTION /usr/local/tomcat
ENV CATALINA_HOME $TOMCAT_FULL_DISTRIBUTION/domibus

RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install -y unzip
RUN apt-get install -y curl

RUN wget https://ec.europa.eu/cefdigital/artifact/repository/eDelivery/eu/domibus/domibus-distribution/$DOMIBUS_VERSION/domibus-distribution-$DOMIBUS_VERSION-tomcat-full.zip
RUN unzip -d $TOMCAT_FULL_DISTRIBUTION domibus-distribution-$DOMIBUS_VERSION-tomcat-full.zip

#RUN wget https://ec.europa.eu/cefdigital/artifact/repository/eDelivery/eu/domibus/domibus-distribution/$DOMIBUS_VERSION/domibus-distribution-$DOMIBUS_VERSION-sample-configuration-and-testing.zip
#RUN unzip -d $DOMIBUS_DIST domibus-distribution-$DOMIBUS_VERSION-sample-configuration-and-testing.zip

RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz
RUN wget https://dev.mysql.com/get/Downloads/Connector-J/$MYSQL_CONNECTOR.zip \
    && unzip -d $DOMIBUS_DIST $MYSQL_CONNECTOR.zip

RUN cp $DOMIBUS_DIST/$MYSQL_CONNECTOR/$MYSQL_CONNECTOR.jar $CATALINA_HOME/lib

RUN chmod 777 $CATALINA_HOME/bin/*.sh
RUN sed -i 's/\r$//' $CATALINA_HOME/bin/setenv.sh
RUN sed -i 's/#export CATALINA_HOME=<YOUR_INSTALLATION_PATH>/sleep 120;/g' $CATALINA_HOME/bin/setenv.sh
RUN sed -i 's/#JAVA_OPTS/JAVA_OPTS/g' $CATALINA_HOME/bin/setenv.sh
RUN sed -i 's/4096/1024/g' $CATALINA_HOME/bin/setenv.sh
RUN sed -i "s/localhost:3306/mysql:3306/g" $CATALINA_HOME/conf/domibus/domibus.properties
RUN sed -i "s/serverName=localhost/serverName=mysql/g" $CATALINA_HOME/conf/domibus/domibus.properties
RUN echo "s/localhost:3306/mysql:3306/g"

######## Set Test Platform certificates ########
RUN mkdir $CATALINA_HOME/conf/domibus/keystores
RUN cp $DOMIBUS_DIST/conf/domibus/keystores/* $CATALINA_HOME/conf/domibus/keystores/
#RUN cp signOnly.xml $CATALINA_HOME/conf/domibus/policies/
#RUN cp doNothingPolicy.xml $CATALINA_HOME/conf/domibus/policies/

RUN echo '#!/bin/bash' >> $DOMIBUS_DIST/entrypoint.sh
RUN echo "cd $CATALINA_HOME" >> $DOMIBUS_DIST/entrypoint.sh
RUN echo './bin/catalina.sh start' >> $DOMIBUS_DIST/entrypoint.sh
RUN echo "while true ; do sleep 1 ; done;" >> $DOMIBUS_DIST/entrypoint.sh
RUN chmod 777 $DOMIBUS_DIST/entrypoint.sh
EXPOSE 8080

WORKDIR $DOMIBUS_DIST
ENTRYPOINT ["./entrypoint.sh"]