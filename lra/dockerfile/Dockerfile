FROM redhat-openjdk-18/openjdk18-openshift
MAINTAINER Ondrej Chaloupka <ochaloup@redhat.com>
EXPOSE 8080

# using jboss user as one java jar is started under
USER root
RUN mkdir /data
RUN chown -R jboss /data \
  && usermod -g root -G `id -g jboss` jboss \
  && chmod -R "g+rwX" /data \
  && chown -R jboss:root /data
USER jboss

ARG NARAYANA_VERSION=5.7.1.Final
RUN curl -LOk https://www.jboss.org/jbosstm/downloads/${NARAYANA_VERSION}/binary/narayana-full-${NARAYANA_VERSION}-bin.zip \
    && unzip -j narayana-full-${NARAYANA_VERSION}-bin.zip narayana-full-${NARAYANA_VERSION}/rts/lra/lra-coordinator-swarm.jar -d /deployments/ \
    && rm narayana-full-$NARAYANA_VERSION-bin.zip

# expected values INFO, DEBUG, TRACE...
ENV LOG_LEVEL="INFO"
# path to the data directory (persistent volume)
ENV OBJECT_STORE_PATH="../../data"
ADD run-java-options /opt/

ENV AB_ENABLED off
ENV JAVA_APP_JAR lra-coordinator-swarm.jar
