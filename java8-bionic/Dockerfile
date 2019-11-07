FROM pufferpanel/pufferd:bionic

RUN apt-get update && \
    apt-get install -y openjdk-8-jdk openjdk-8-jre && \
    apt-get clean && \
    java -version && \
    javac -version