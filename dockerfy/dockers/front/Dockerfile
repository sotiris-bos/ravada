FROM ubuntu:18.04
LABEL maintainer="ravada@telecos.upc.edu"
LABEL description="Ravada Frontend"
ARG DEBIAN_FRONTEND=noninteractive
ENV RAVADA_BRANCH="develop"


RUN apt-get update \
 &&  apt-get install -y -q --no-install-recommends \
    perl libmojolicious-perl libauthen-passphrase-perl \
    libdbd-mysql-perl libdbi-perl libdbix-connector-perl libipc-run3-perl libnet-ldap-perl \
    libproc-pid-file-perl libsys-virt-perl libxml-libxml-perl libconfig-yaml-perl \
    libmoose-perl libjson-xs-perl perlmagick libmoosex-types-netaddr-ip-perl libsys-statistics-linux-perl \
    libio-interface-perl libiptables-chainmgr-perl libnet-dns-perl liblocale-maketext-lexicon-perl \
    libmojolicious-plugin-i18n-perl libdbd-sqlite3-perl debconf adduser libdigest-sha-perl libnet-ssh2-perl libpbkdf2-tiny-perl \
    libfile-rsync-perl libdate-calc-perl libparallel-forkmanager-perl libdatetime-perl libencode-locale-perl netcat-openbsd \
    libio-stringy-perl libvirt-clients liblwp-useragent-determined-perl supervisor net-tools apt-utils lsof mysql-client \
	curl bash vim wget \
    tzdata unzip \
 && apt-get clean && \
   echo "**** download ravada files and copy to directories ****" && \
   mkdir /ravada && \
   curl -o \
   /tmp/ravada.zip -L \
   "https://github.com/UPC/ravada/archive/${RAVADA_BRANCH}.zip" && \
   unzip /tmp/ravada.zip -d /tmp/ravadatmp && \
   cp -r /tmp/ravadatmp/ravada-develop/* /ravada && \
   cp /ravada/dockerfy/dockers/front/ravada.conf /etc/ && \
   cp /ravada/dockerfy/dockers/front/supervisord.conf /etc/ \
 && rm -rf \
   /var/lib/apt/lists/* \
   /tmp/*

ADD http://infoteleco.upc.edu/img/debian/libmojolicious-plugin-renderfile-perl_0.10-1_all.deb /tmp/
RUN dpkg -i /tmp/libmojolicious-plugin-renderfile-perl_0.10-1_all.deb \
 && rm /tmp/libmojolicious-plugin-renderfile-perl_0.10-1_all.deb \
 && mkdir -p /var/log/supervisor \
 && mkdir -p /run/sshd

EXPOSE 3000

ENV PERL5LIB /root/local/lib/perl5:/root/lib/:/ravada




WORKDIR /ravada

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
