FROM confluentinc/cp-kafka-connect:4.1.1

ADD jars /jars
RUN cp /jars/exasol-jdbc-6.0.8.jar /usr/share/java/kafka-connect-jdbc/
RUN cp /jars/kafka-connect-jdbc-5.1.0-SNAPSHOT.jar /usr/share/java/kafka-connect-jdbc/
RUN rm /usr/share/java/kafka-connect-jdbc/kafka-connect-jdbc-4.1.1.jar
