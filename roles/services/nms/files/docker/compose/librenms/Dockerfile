FROM jarischaefer/docker-librenms:latest

# Install check_nrpe
RUN set -ex \
    && apt-get update \
    && apt-get -y install --no-install-recommends nagios-nrpe-plugin \
    && apt-get -y autoremove --purge \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && find /var/log -type f | while read f; do echo -ne '' > $f; done;

# Install Weathermapper plugin
RUN set -ex \
    && cd /opt \
    && git clone https://github.com/pblasquez/weathermapper.git \
    && chown -R librenms:librenms /opt/weathermapper \
    && cp -pR /opt/weathermapper/librenms_plugin/Weathermapper /opt/librenms/html/plugins/

# RRDCached fix for Smokeping integration
RUN set -ex \
    && sed -i ':a;N;$!ba;s/\t-B \\\n//g' /etc/service/rrdcached/run \
    && sed -i ':a;N;$!ba;s/\t-R \\\n//g' /etc/service/rrdcached/run

# Add cronjob for smokeping:generate --targets
RUN set -ex \
    && echo "0	*	*	*	*	librenms	. /etc/librenms_environment; /opt/librenms/lnms smokeping:generate --targets > /opt/smokeping/config/librenms-targets.conf" >> /etc/cron.d/librenms
