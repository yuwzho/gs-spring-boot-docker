FROM ubuntu

RUN add-apt-repository -y ppa:openjdk-r/ppa \
  && apt-get update \
  && apt-get install -y openjdk-8-jre
VOLUME /tmp
ARG JAR_FILE
ADD ${JAR_FILE} app.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]

ENV FILEBEAT_VERSION 5.4.1

RUN apt-get update && \
    apt-get -y install wget && \
    wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-${FILEBEAT_VERSION}-linux-x86_64.tar.gz && \
    echo "$(wget -qO - https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-${FILEBEAT_VERSION}-linux-x86_64.tar.gz.sha1) filebeat-${FILEBEAT_VERSION}-linux-x86_64.tar.gz" | sha1sum -c - && \
    tar xzf filebeat-${FILEBEAT_VERSION}-linux-x86_64.tar.gz && \
    mv filebeat-${FILEBEAT_VERSION}-linux-x86_64/filebeat /usr/local/bin && \
    rm -rf /filebeat* && \
    apt-get -y remove wget && \
    apt-get -y autoremove

RUN apt-get -y install curl

COPY filebeat.yml /etc/filebeat/

CMD ["/usr/local/bin/filebeat", "-e", "-c", "/etc/filebeat/filebeat.yml"]