FROM pufferpanel/pufferd:alpine

ENV JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk
ENV PATH="$JAVA_HOME/bin:${PATH}"

RUN apk add --no-cache openjdk8 && \
    java -version && \
    javac -version