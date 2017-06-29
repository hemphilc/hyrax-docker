###
# Dockerfile for Hyrax BES
###

FROM centos:7

MAINTAINER support@opendap.org

USER root

# Recommended maintenance
RUN \
    yum -y install which && \
    yum -y update && \
    yum clean all 


# Get The Supervisor (First get pip, install pip, then use pip to install supervisor.
RUN \
    curl https://bootstrap.pypa.io/get-pip.py -o /get-pip.py && \
    python /get-pip.py && \
    pip install supervisor && \
    echo "supervisord " `supervisord --version`

COPY supervisord.conf /etc

# Install Hyrax from RPM
ENV HYRAX_VERSION=1.13.4
RUN yum -y install https://www.opendap.org/pub/binary/hyrax-1.13.4/centos-7.x/libdap-3.19.0-1.el7.centos.x86_64.rpm \
                   https://www.opendap.org/pub/binary/hyrax-1.13.4/centos-7.x/bes-3.18.0-1.static.el7.centos.x86_64.rpm
RUN which besdaemon


COPY entrypoint.sh /
RUN  chmod +x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

EXPOSE 10022
EXPOSE 11002

# can't use USER with entrypoint that needs root
# use gosu or, as done, enable bes user write so the entrypoint doe snot need root
RUN  chown -R bes /etc/bes
USER bes

# tunables - to be overridden on command line or in docker-compose or ansible
ENV BES_HELP_EMAIL=help@replaceme
ENV BES_SYMLINKS=Yes
# Yes (hyrax docker default overrides rpm default) or No, for symlinks in /usr/share/hyrax


RUN echo "############################" && \
    ls -l /usr/bin/besdaemon /usr/bin/supervisord /etc/supervisord.conf && \
    echo "############################" 


CMD ["-"]

