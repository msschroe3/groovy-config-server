FROM frolvlad/alpine-oraclejdk8:slim
VOLUME /tmp
ADD /target/config-server-1.0.0.jar opt/app.jar
ENV JAVA_OPTS=""
ENTRYPOINT [ "sh", "-c", "java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar opt/app.jar" ]