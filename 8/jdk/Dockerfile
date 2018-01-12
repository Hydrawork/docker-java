# AlpineLinux with Oracle Java 8
FROM hydrawork/alpine:3.6

MAINTAINER Alex Huang <alex@hydra.work>

# Java Version and other ENV
ENV JAVA_HOME=/opt/java \
    PATH=${PATH}:/opt/java/bin \
    JAVA_VERSION_MAJOR=8 \
    JAVA_VERSION_MINOR=152 \
    JAVA_VERSION_BUILD=16 \
    JAVA_VERSION_SOLT=aa0333dd3019491ca4f6ddbe78cdb6d0 \
    JAVA_PACKAGE=jdk

# do all in one step
RUN apk add --update --no-cache bash ca-certificates curl && \
    mkdir /opt && \
    curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie" -o /tmp/java.tar.gz \
      http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/${JAVA_VERSION_SOLT}/${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz && \
    JAVA_PACKAGE_SHA256=$(curl -sSL https://www.oracle.com/webfolder/s/digest/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}checksum.html | grep -E "${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64\.tar\.gz" | grep -Eo '(sha256: )[^<]+' | cut -d: -f2 | xargs) && \
    echo "${JAVA_PACKAGE_SHA256}  /tmp/java.tar.gz" > /tmp/java.tar.gz.sha256 && \
    sha256sum -c /tmp/java.tar.gz.sha256 && \
    # Install jdk
    gunzip /tmp/java.tar.gz && \
    tar -C /opt -xf /tmp/java.tar && \
    mv /opt/jdk* $JAVA_HOME && \
    find $JAVA_HOME/ -maxdepth 1 -mindepth 1 | grep -v jre | grep -v include | grep -v bin | grep -v lib | xargs rm -rf && \
    # Install jce
    curl -jksSLH "Cookie: oraclelicense=accept-securebackup-cookie" -o /tmp/jce_policy-${JAVA_VERSION_MAJOR}.zip \
        http://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION_MAJOR}/jce_policy-${JAVA_VERSION_MAJOR}.zip && \
    cd /tmp && unzip /tmp/jce_policy-${JAVA_VERSION_MAJOR}.zip && \
    cp -v /tmp/UnlimitedJCEPolicy*/*.jar $JAVA_HOME/jre/lib/security && \
    sed -i s/#networkaddress.cache.ttl=-1/networkaddress.cache.ttl=10/ $JAVA_HOME/jre/lib/security/java.security && \
    sed -i /^securerandom\.source\=file\:/s@/dev/urandom@/dev/./urandom@ $JAVA_HOME/jre/lib/security/java.security && \
    # Reduce package and clean cache
    apk del curl && \
    rm -rf $JAVA_HOME/*src.zip \
           $JAVA_HOME/bin/appletviewer \
           $JAVA_HOME/bin/extcheck \
           $JAVA_HOME/bin/pack200 \
           $JAVA_HOME/bin/unpack200 \
           $JAVA_HOME/bin/ws* \
           $JAVA_HOME/lib/missioncontrol \
           $JAVA_HOME/lib/visualvm \
           $JAVA_HOME/lib/*javafx* \
           $JAVA_HOME/jre/plugin \
           $JAVA_HOME/jre/bin/javaws \
           $JAVA_HOME/jre/bin/jjs \
           $JAVA_HOME/jre/bin/orbd \
           $JAVA_HOME/jre/bin/pack200 \
           $JAVA_HOME/jre/bin/policytool \
           $JAVA_HOME/jre/bin/rmid \
           $JAVA_HOME/jre/bin/rmiregistry \
           $JAVA_HOME/jre/bin/servertool \
           $JAVA_HOME/jre/bin/tnameserv \
           $JAVA_HOME/jre/bin/unpack200 \
           $JAVA_HOME/jre/lib/javaws.jar \
           $JAVA_HOME/jre/lib/deploy* \
           $JAVA_HOME/jre/lib/desktop \
           $JAVA_HOME/jre/lib/*javafx* \
           $JAVA_HOME/jre/lib/*jfx* \
           $JAVA_HOME/jre/lib/amd64/libdecora_sse.so \
           $JAVA_HOME/jre/lib/amd64/libprism_*.so \
           $JAVA_HOME/jre/lib/amd64/libfxplugins.so \
           $JAVA_HOME/jre/lib/amd64/libglass.so \
           $JAVA_HOME/jre/lib/amd64/libgstreamer-lite.so \
           $JAVA_HOME/jre/lib/amd64/libjavafx*.so \
           $JAVA_HOME/jre/lib/amd64/libjfx*.so \
           $JAVA_HOME/jre/lib/ext/jfxrt.jar \
           $JAVA_HOME/jre/lib/ext/nashorn.jar \
           $JAVA_HOME/jre/lib/oblique-fonts \
           $JAVA_HOME/jre/lib/plugin.jar \
           /tmp/* /var/cache/apk/* && \
    echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf

# EOF
