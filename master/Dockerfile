FROM oberthur/docker-ubuntu-java:jdk8_8.71.15

MAINTAINER Dawid Malinowski <d.malinowski@oberthur.com>

ENV JENKINS_VERSION 1.642.1

RUN curl -L https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add - \
    && sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list' \
    && apt-get update \
    && apt-get install -y jenkins=$JENKINS_VERSION supervisor \
    # clean all cache to clean space
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean \
    && echo '#!/bin/bash -x\n\njava -Djava.awt.headless=true -DJENKINS_HOME=/var/lib/jenkins -jar /usr/share/jenkins/jenkins.war --logfile=/var/log/jenkins/jenkins.log --webroot=/var/cache/jenkins/war --httpPort=8080 --prefix=/jenkins' > /usr/bin/jenkins_master.sh \
    && chmod 755 /usr/bin/jenkins_master.sh && echo "[program:jenkins-master]\ncommand=/usr/bin/jenkins_master.sh\nuser=jenkins" > /etc/supervisor/conf.d/supervisor.conf

ENTRYPOINT [ "/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf" ]
