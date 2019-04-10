FROM tomcat:9-jre11

ENV GEOSERVER_VERSION 2.15-SNAPSHOT
ENV GEOSERVER_DATA_DIR /var/local/geoserver
ENV GEOSERVER_INSTALL_DIR /usr/local/broker

ENV BROKER_FILE geoserver-${GEOSERVER_VERSION}-activeMQ-broker-plugin.zip
ENV BROKER_URL https://build.geoserver.org/geoserver/2.15.x/community-latest/${BROKER_FILE}


# GeoServer
#ADD conf/geoserver.xml /usr/local/tomcat/conf/Catalina/localhost/geoserver.xml

ADD ./geoserver-2.15-SNAPSHOT-activeMQ-broker-plugin.zip /service.zip

RUN mkdir -p /usr/local/tomcat/conf/Catalina/localhost && echo "<Context path=\"/broker\" docBase=\"/usr/local/broker\"></Context>" > /usr/local/tomcat/conf/Catalina/localhost/broker.xml \
        && mkdir ${GEOSERVER_DATA_DIR} \
	&& mkdir ${GEOSERVER_INSTALL_DIR} \
	&& cd ${GEOSERVER_INSTALL_DIR} \
	&& unzip /service.zip \
	&& unzip activemqBroker-${GEOSERVER_VERSION}.war \
	&& rm *.war /service.zip
#RUN mkdir -p /usr/local/tomcat/conf/Catalina/localhost && echo "<Context path=\"/broker\" docBase=\"/usr/local/broker\"></Context>" > /usr/local/tomcat/conf/Catalina/localhost/broker.xml \
       # && mkdir ${GEOSERVER_DATA_DIR} \
#	&& mkdir ${GEOSERVER_INSTALL_DIR} \
#	&& cd ${GEOSERVER_INSTALL_DIR} \
#	&& wget ${BROKER_URL} \
#	&& unzip ${BROKER_FILE} \
#	&& unzip activemqBroker-${GEOSERVER_VERSION}.war \
#	&& rm *.war *.zip

# Tomcat environment
ENV CATALINA_OPTS "-server -Djava.awt.headless=true \
	-Xms768m -Xmx1560m -XX:+UseConcMarkSweepGC -XX:NewSize=48m \
	-DGEOSERVER_DATA_DIR=${GEOSERVER_DATA_DIR} \
        -Dactivemq.transportConnectors.server.uri=\"tcp://0.0.0.0:61666?maximumConnections=1000&wireFormat.maxFrameSize=104857600&jms.useAsyncSend=true&transport.daemon=true\""

RUN apt-get update && apt-get install telnet

CMD catalina.sh run

EXPOSE 61666
EXPOSE 8080
