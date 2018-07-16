FROM confluentinc/cp-kafka-connect:4.1.1

ADD jars /opt/kafka-connect/jars
RUN cp /opt/kafka-connect/jars/exasol-jdbc-6.0.8.jar /usr/share/java/kafka-connect-jdbc/
RUN cp /opt/kafka-connect/jars/kafka-connect-jdbc-5.1.0-SNAPSHOT.jar /usr/share/java/kafka-connect-jdbc/
RUN rm /usr/share/java/kafka-connect-jdbc/kafka-connect-jdbc-4.1.1.jar
