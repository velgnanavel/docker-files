FROM maven:3.2.5-jdk-7

MAINTAINER Xinglang Wang <wangxinglang@gmail.com>

# Set default encoding to UTF-8
RUN echo "LANG=C.UTF-8" > /etc/default/locale

# Build
RUN mkdir -p /opt/app
WORKDIR /opt
RUN git clone https://github.com/pulsarIO/realtime-analytics.git
WORKDIR /opt/realtime-analytics/
RUN git checkout realtime-analytics-1.0.2
WORKDIR /opt/realtime-analytics/Demo/twittersample
RUN mvn -DskipTests clean install

ENV project_version 1.0.2
ENV project_name twittersample

RUN cp target/${project_name}-${project_version}-bin.tar.gz /opt/app/${project_name}-${project_version}-bin.tar.gz
WORKDIR /opt/app
RUN tar -zxvf ${project_name}-${project_version}-bin.tar.gz
RUN ln -s /opt/app/${project_name}-${project_version} jetstreamapp
WORKDIR /opt/app/jetstreamapp

# App config
ENV JETSTREAM_APP_JAR_NAME ${project_name}.jar
ENV JETSTREAM_APP_NAME ${project_name}
ENV JETSTREAM_CONFIG_VERSION 1.0

# Dependency
ENV JETSTREAM_ZKSERVER_HOST zkserver
ENV JETSTREAM_ZKSERVER_PORT 2181
ENV JETSTREAM_MONGOURL mongo://mongoserver:27017/config

#Suggested production JAVA_OPS for Open JDK.
#ENV JETSTREAM_JAVA_OPTS -server -Xms6g -Xmx6g -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+UseCompressedOops -X:MaxTenuringThreshold=8 -XX:CMSInitiatingOccupancyFraction=75 -XX:MaxNewSize=3g -XX:NewSize=3g -XX:+CMSConcurrentMTEnabled -XX:+CMSScavengeBeforeRemark 

# One context port
ENV JETSTREAM_CONTEXT_BASEPORT 15590
ENV JETSTREAM_APP_PORT 9999

EXPOSE 9999
ENTRYPOINT ./start.sh
