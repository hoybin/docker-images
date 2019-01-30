ARG MYSQL_BASE_IMAGE_VERSION
FROM mysql:${MYSQL_BASE_IMAGE_VERSION}

LABEL maintainer="hoybin@outlook.com"

COPY image-files/ /

VOLUME ["/etc/mysql/conf.d", "/docker-entrypoint-initdb.d", "/var/lib/mysql"]
