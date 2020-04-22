ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base

COPY rootfs/ /

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y cron nginx-extras jitsi-meet-web && \
	apt-dpkg-wrap apt-get install -y -t stretch-backports certbot && \
	apt-dpkg-wrap apt-get -d install -y jitsi-meet-web-config && \
	dpkg -x /var/cache/apt/archives/jitsi-meet-web-config*.deb /tmp/pkg && \
	mv /tmp/pkg/usr/share/jitsi-meet-web-config/config.js /defaults && \
	mv /usr/share/jitsi-meet/interface_config.js /defaults && \
	apt-cleanup && \
	rm -f /etc/nginx/conf.d/default.conf && \
	rm -rf /tmp/pkg /var/cache/apt  && \
	apt update && apt install -y git make gcc curl software-properties-common && \
	curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
	apt-get install -y nodejs  && \
	mv /usr/share/jitsi-meet /usr/share/jitsi-meet.org && \
	git clone https://github.com/jitsi/jitsi-meet.git /usr/share/jitsi-meet && \
	sed -i "/^export function isMobileBrowser.*/areturn Platform.OS === 'ios';" /usr/share/jitsi-meet/react/features/base/environment/utils.js && \
	cd /usr/share/jitsi-meet && npm install && make && \
	rm -rf /usr/share/jitsi-meet/node_modules && rm -rf /usr/share/jitsi-meet/ios && apt-cleanup

EXPOSE 80 443

VOLUME ["/config", "/etc/letsencrypt", "/usr/share/jitsi-meet/transcripts"]

#	sed -i '/^export function isMobileBrowser.*/areturn false;' /usr/share/jitsi-meet/react/features/base/environment/utils.js && \

