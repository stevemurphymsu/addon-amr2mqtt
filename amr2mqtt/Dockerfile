ARG BUILD_FROM
FROM $BUILD_FROM

WORKDIR /usr/src
ARG BUILD_ARCH
ENV RTLAMR_VERSION=0.9.1

RUN set -eux \
    && apk update \
	&& apk add --no-cache --virtual .build-deps \
            tar \
            curl \
			git \
			build-base \
			cmake \
			libusb-dev \
			eudev \ 
			udev-init-scripts \
    && git clone https://github.com/rtlsdrblog/rtl-sdr-blog \
    && cd rtl-sdr-blog \
	&& mkdir build \
	&& cd build \
	&& cmake ../ -DINSTALL_UDEV_RULES=ON \
	&& make \
	&& make DESTDIR=/usr install \
	&& cp ../rtl-sdr.rules /etc/udev/rules.d/ \
	&& cd ../.. \
    && curl -J -L -o /tmp/rtlamr.tar.gz \
    "https://github.com/bemasher/rtlamr/releases/download/v${RTLAMR_VERSION}/rtlamr_linux_${BUILD_ARCH}.tar.gz" \
    && tar -xf /tmp/rtlamr.tar.gz -C /usr/bin \
    && chmod a+x /usr/bin/rtlamr \
    && rm /tmp/rtlamr.tar.gz \
    && echo "Add user for AMR2MQTT" \
    && mkdir -p /data/amr2mqtt \
	&& apk del .build-deps \
			git \
			build-base \
			cmake \
    && rm -rf /usr/src/*

COPY rootfs /
