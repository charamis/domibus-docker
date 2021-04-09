FROM mysql:8

ENV DOMIBUS_VERSION 4.2.1
ENV SQL_SCRIPTS_URL https://ec.europa.eu/cefdigital/artifact/repository/eDelivery/eu/domibus/domibus-distribution/$DOMIBUS_VERSION/domibus-distribution-$DOMIBUS_VERSION-sql-scripts.zip
ENV SQL_SCRIPTS_DIR /usr/local/domibus-mysql

RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install unzip

RUN wget $SQL_SCRIPTS_URL && unzip -d $SQL_SCRIPTS_DIR domibus-distribution-$DOMIBUS_VERSION-sql-scripts.zip
RUN cp $SQL_SCRIPTS_DIR/sql-scripts/mysql-$DOMIBUS_VERSION.ddl /docker-entrypoint-initdb.d/mysql-$DOMIBUS_VERSION.sql

EXPOSE 3306