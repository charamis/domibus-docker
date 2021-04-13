FROM mysql:8

ENV DEBIAN_FRONTEND noninteractive

ENV SQLSCRIPTS_VERSION 4.2.1

ENV DOMIBUS_VERSION 4.2.1

ENV SQL_SCRIPTS_URL ./domibus-distribution-$DOMIBUS_VERSION-sql-scripts.zip
ENV SQL_SCRIPTS_HOME /usr/local/domibusSql

RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install -y unzip

RUN wget https://ec.europa.eu/cefdigital/artifact/repository/eDelivery/eu/domibus/domibus-distribution/$DOMIBUS_VERSION/domibus-distribution-$DOMIBUS_VERSION-sql-scripts.zip

RUN unzip  -d $SQL_SCRIPTS_HOME domibus-distribution-$DOMIBUS_VERSION-sql-scripts.zip
RUN cp $SQL_SCRIPTS_HOME/sql-scripts/mysql-$SQLSCRIPTS_VERSION.ddl /docker-entrypoint-initdb.d/mysql-$SQLSCRIPTS_VERSION.sql

EXPOSE 3306