FROM centos:7

#Set The Variables
ENV NESSUS_VERSION="7.0.3"
ENV NESSUS_NAME="Nessus_Docker_Container"
ENV NESSUS_KEY="Get from https://cloud.tenable.com/app.html#/scans/scanners"

#Update The Container
RUN set -x
RUN yum -y update nss-util bind-license libssh2 && yum clean all && rm -fr /var/cache/yum
RUN yum -y install expect

# Import Tanable's GPG key
RUN rpm --import https://static.tenable.com/marketing/RPM-GPG-KEY-Tenable

RUN  DOWNLOAD_ID=$(curl -ssl -o - "https://www.tenable.com/downloads/nessus" | sed -n -e 's/.*data-download-id="\([0-9]*\)".*data-file-name="\([a-zA-Z0-9_\.-]\+\-es7\.x86_64\.rpm\).*".*/\1/p') && curl -ssL -o /tmp/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm "https://tenable-downloads-production.s3.amazonaws.com/uploads/download/file/${DOWNLOAD_ID}/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm"

RUN (yum -y localinstall /tmp/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm || \
    yum -y localinstall /tmp/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm) && \
    yum clean all

# Cleanup
RUN rm /tmp/Nessus-${NESSUS_VERSION}-es7.x86_64.rpm \
    && yum clean all \
    && rm -rf /var/cache/yum \
    && rm -rf /opt/nessus/var/nessus/{uuid,*.db*,master.key}

# Add NESSUS_USER
COPY add_user.exp /
RUN expect /add_user.exp

# Set HOSTNAME and Register
RUN /opt/nessus/sbin/nessuscli managed link --key=${NESSUS_KEY} --name=${NESSUS_NAME} --cloud

EXPOSE 8834
CMD ["/opt/nessus/sbin/nessus-service"]
