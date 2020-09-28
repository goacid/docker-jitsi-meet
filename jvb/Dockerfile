ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base-java

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jitsi-videobridge2 jq curl iproute2 && \
	apt-cleanup

COPY rootfs/ /

RUN cd /tmp && \ 
	wget http://security.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u12_amd64.deb  && \
	dpkg -i libssl1.0.0_1.0.1t-1+deb8u12_amd64.deb && \
	rm libssl1.0.0_1.0.1t-1+deb8u12_amd64.deb

VOLUME /config
