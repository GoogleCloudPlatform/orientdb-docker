FROM gcr.io/google-appengine/openjdk

# This is release version of OrientDB to pull in.
ENV ORIENTDB_VERSION 2.2.37
ENV ORIENTDB_SHA1 dd85f8c22ba850d78809e43fe249e0a1e985b1fd

ENV C2D_RELEASE 2.2.37

ENV ORIENTDB_DOWNLOAD_SERVER https://api.github.com/repos/orientechnologies/orientdb/tarball/

ENV ORIENTDB_DOWNLOAD_URL ${ORIENTDB_DOWNLOAD_SERVER}/${ORIENTDB_VERSION}

ENV ORIENTDB_HOME /orientdb

RUN set -x \
    && apt update \
    && apt install -y curl wget \
    && rm -rf /var/lib/apt/lists/*

# Download distribution tar, untar and delete databases.
RUN set -x \
  && mkdir ${ORIENTDB_HOME} \
  && wget ${ORIENTDB_DOWNLOAD_URL} -O ${ORIENTDB_VERSION}.tar.gz \
  && echo "${ORIENTDB_SHA1} ${ORIENTDB_VERSION}.tar.gz" | sha1sum -c - \
  && tar -xvzf ${ORIENTDB_VERSION}.tar.gz -C ${ORIENTDB_HOME} --strip-components=1 \
  && rm ${ORIENTDB_VERSION}.tar.gz \
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
