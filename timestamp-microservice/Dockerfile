FROM momozor/sbcl

# persistent / runtime deps
RUN apt-get update && apt-get install -y \
		ca-certificates \
		git \
	--no-install-recommends && rm -r /var/lib/apt/lists/*

ENV GPG_KEYS D7A3489DDEFE32B7D0E7CC61307965AB028B5FF7

ENV QUICKLISP_LISP_URL https://beta.quicklisp.org/quicklisp.lisp
# ENV QUICKLISP_LISP_ASC_URL https://beta.quicklisp.org/quicklisp.lisp.asc
ENV QUICKLISP_LISP_SHA256 4a7a5c2aebe0716417047854267397e24a44d0cce096127411e9ce9ccfeb2c17

RUN set -eux; \
	\
	fetchDeps=' \
		wget \
	'; \
	if ! command -v gpg > /dev/null; then \
		fetchDeps="$fetchDeps \
			dirmngr \
			gnupg \
		"; \
	fi; \
	apt-get update; \
	apt-get install -y --no-install-recommends $fetchDeps; \
	rm -rf /var/lib/apt/lists/*; \
	\
	wget -O quicklisp.lisp "$QUICKLISP_LISP_URL"; \
	\
	if [ -n "${QUICKLISP_LISP_SHA256-}" ]; then \
		echo "$QUICKLISP_LISP_SHA256 *quicklisp.lisp" | sha256sum -c -; \
	fi; \
	\
	if [ -n "${QUICKLISP_LISP_ASC_URL-}" ]; then \
		wget -O quicklisp.lisp.asc "$QUICKLISP_LISP_ASC_URL"; \
		export GNUPGHOME="$(mktemp -d)"; \
		for key in $GPG_KEYS; do \
			gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
		done; \
		gpg --batch --verify quicklisp.lisp.asc quicklisp.lisp; \
		command -v gpgconf > /dev/null && gpgconf --kill all; \
		rm -rf "$GNUPGHOME"; \
	fi; \
	\
	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false $fetchDeps

COPY install.lisp ./
RUN sbcl --load install.lisp

WORKDIR /root/quicklisp/local-projects/timestamp-microservice

RUN git clone https://github.com/momozor/FCC-timestamp-microservice.git .

CMD ["sbcl", "--load", "app.lisp", "--eval", "(ql:quickload :timestamp-microservice)", "--eval", "(timestamp-microservice:start :port (parse-integer (uiop:getenv \"PORT\")))"]
