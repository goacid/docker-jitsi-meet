ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base

ADD https://prosody.im/files/prosody-debian-packages.key /tmp/prosody.key

RUN \
    apt-key add /tmp/prosody.key \
    && rm -f /tmp/prosody.key \
    && echo "deb http://packages.prosody.im/debian stretch main" > /etc/apt/sources.list.d/prosody.list \
    && apt-dpkg-wrap apt-get update \
    && apt-dpkg-wrap apt-get install -y prosody \
    && apt-dpkg-wrap apt-get install -t stretch-backports -y \
      liblua5.2-dev \
      sasl2-bin \
      libsasl2-modules-ldap \
      libsasl2-dev \
      libssl1.0-dev \
      lua-basexx \
      lua-ldap \
      lua-sec \
      luarocks \
      git \
      gcc \
      patch \
    && luarocks install cyrussasl 1.1.0-1 \
    && luarocks install lua-cjson 2.1.0-1 \
    && luarocks install luajwtjitsi 1.3-7 \
    && luarocks install net-url 0.9-1 \
    && apt-dpkg-wrap apt-get remove -t stretch-backports -y \
      git \
      gcc \
      luarocks \
      libsasl2-dev \
      libssl1.0-dev \
      liblua5.2-dev \
    && apt-cleanup \
    && rm -rf /etc/prosody /var/cache/apt

RUN \
    apt-dpkg-wrap apt-get update \
    && apt-dpkg-wrap apt-get -d install -y jitsi-meet-prosody \
    && dpkg -x /var/cache/apt/archives/jitsi-meet-prosody*.deb /tmp/pkg \
    && mv /tmp/pkg/usr/share/jitsi-meet/prosody-plugins /prosody-plugins \
    && apt-cleanup \
    && rm -rf /tmp/pkg /var/cache/apt

RUN patch -d /usr/lib/prosody/modules/muc -p0 < /prosody-plugins/muc_owner_allow_kick.patch

RUN wget -O /tmp/http.patch https://github.com/bjc/prosody/commit/a56dab0d87d19c507f66bf4b8284aa000b81d334.patch
RUN patch -d /usr/lib/prosody/ -p1 < /tmp/http.patch
RUN wget -O /tmp/luajwtjitsi.lua.patch https://gist.githubusercontent.com/goacid/43b804387ff3428762d633c0f305a9a2/raw/94731cec0605ce9769f8c519066e92369ab9f14e/luajwtjitsi.lua.patch
RUN patch -d /usr/local/share/lua/5.2 -p1 < /tmp/luajwtjitsi.lua.patch

COPY rootfs/ /

EXPOSE 5222 5269 5347 5280

VOLUME ["/config", "/prosody-plugins-custom"]