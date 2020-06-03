FROM gcr.io/google-appengine/openjdk

# This is release version of OrientDB to pull in.
ENV ORIENTDB_VERSION 3.1.0
ENV ORIENTDB_SHA1 87fb84959d1db9d9ca2277df34d7114b80446e0d

ENV C2D_RELEASE 3.1.0

ENV ORIENTDB_DOWNLOAD_SERVER https://repo1.maven.org/maven2/com/orientechnologies

ENV ORIENTDB_DOWNLOAD_URL ${ORIENTDB_DOWNLOAD_SERVER}/orientdb-community/${ORIENTDB_VERSION}/orientdb-community-${ORIENTDB_VERSION}.tar.gz

ENV ORIENTDB_HOME /orientdb

RUN set -x \
    && apt update \
    && apt install -y curl wget \
    && rm -rf /var/lib/apt/lists/*

# Download distribution tar, untar and delete databases.
RUN set -x \
  && mkdir ${ORIENTDB_HOME} \
  && wget  ${ORIENTDB_DOWNLOAD_URL} \
  && echo "${ORIENTDB_SHA1} *orientdb-community-${ORIENTDB_VERSION}.tar.gz" | sha1sum -c - \
  && tar -xvzf orientdb-community-${ORIENTDB_VERSION}.tar.gz -C ${ORIENTDB_HOME} --strip-components=1 \
  && rm orientdb-community-${ORIENTDB_VERSION}.tar.gz \
  && rm -rf ${ORIENTDB_HOME}/databases/*

# Copy OrientDB license to container image.
RUN set -x \
    # Converts version string to similar format: 3.0.x
    && export ORIENTDB_LICENSE_VERSION="${ORIENTDB_VERSION%.*}.x" \
    && mkdir -p /usr/share/doc/orientdb \
    && wget -q "https://raw.githubusercontent.com/orientechnologies/orientdb/${ORIENTDB_LICENSE_VERSION}/license.txt" -O /usr/share/doc/orientdb/LICENSE

ENV PATH /orientdb/bin:$PATH

VOLUME ["/orientdb/backup", "/orientdb/databases", "/orientdb/config"]

ENV ORIENTDB_HTTP=2480 \
    ORIENTDB_BINARY=2424

WORKDIR ${ORIENTDB_HOME}

EXPOSE ${ORIENTDB_HTTP} \
       ${ORIENTDB_BINARY}

CMD ["server.sh"]
