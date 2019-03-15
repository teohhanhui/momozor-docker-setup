FROM debian:stretch-slim

ENV SBCL_VERSION=1.5.0
RUN set -eux; \
	\
	fetchDeps=' \
		bzip2 \
		ca-certificates \
		make \
		wget \
	'; \
	apt-get update; \
	apt-get install -y --no-install-recommends $fetchDeps; \
	rm -rf /var/lib/apt/lists/*; \
	\
	wget -O sbcl.tar.bz2 https://downloads.sourceforge.net/project/sbcl/sbcl/$SBCL_VERSION/sbcl-$SBCL_VERSION-x86-64-linux-binary.tar.bz2; \
	\
	mkdir -p /usr/src/sbcl; \
	tar -jxf sbcl.tar.bz2 -C /usr/src/sbcl --strip-components=1; \
	rm sbcl.tar.bz2; \
	\
	cd /usr/src/sbcl; \
	sh install.sh; \
	\
	rm -r /usr/src/sbcl; \
	\
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $fetchDeps \
	\
	sbcl --version

CMD ["sbcl"]
