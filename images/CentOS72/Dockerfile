FROM centos:7.2.1511
MAINTAINER Jon Hadfield <jon@lessknown.co.uk>

COPY . /usr/src/buildbot-worker
COPY docker/buildbot.tac /buildbot/buildbot.tac

# Last build date - this can be updated whenever there are security updates s
# that everything is rebuilt
ENV         security_updates_as_of 2016-08-23

RUN yum update -y && yum install -y epel-release && \ 
    yum install -y deltarpm && \
    yum groupinstall -y "Development Tools" && \
    yum install -y \
    python-devel \
    openssl-devel \
    git \
    subversion \ 
    python-pip && \
    yum clean all

RUN pip install pip -U --no-cache-dir

RUN curl -Lo /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.0.0/dumb-init_1.0.0_amd64
RUN chmod +x /usr/local/bin/dumb-init

# Install required python packages, and twisted
RUN         pip install service_identity pycrypto --no-cache-dir && \
            pip install twisted --no-cache-dir && \
            pip install /usr/src/buildbot-worker --no-cache-dir 


RUN useradd -ms /bin/bash buildbot && chown -R buildbot /buildbot
# USER buildbot

WORKDIR /buildbot

CMD ["/usr/local/bin/dumb-init", "twistd", "-ny", "buildbot.tac"]
