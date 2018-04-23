#+++++++++++++++++++++++++++++++++++++++
# Dockerfile for webdevops/apache:ubuntu-16.04
#    -- automatically generated  --
#+++++++++++++++++++++++++++++++++++++++

FROM webdevops/base:ubuntu-16.04

RUN set -x \
    # Install apache
    && apt-install \
        apache2 \
        -y vim \
        -y curl \
	&& sed -ri ' \
		s!^(\s*CustomLog)\s+\S+!\1 /proc/self/fd/1!g; \
		s!^(\s*ErrorLog)\s+\S+!\1 /proc/self/fd/2!g; \
		' /etc/apache2/apache2.conf \
	&& rm -f /etc/apache2/sites-enabled/* \
	&& ln -sf /opt/docker/etc/httpd/main.conf /etc/apache2/sites-enabled/10-docker.conf \
	&& a2enmod actions proxy proxy_fcgi ssl rewrite headers expires \
    && docker-run-bootstrap \
    && docker-image-cleanup

ARG PHP_SOCKET=php-fpm:9000

ENV WEB_PHP_SOCKET=$PHP_SOCKET
ENV WEB_DOCUMENT_ROOT=/var/www/html
ENV APACHE_LOG_DIR /var/log/apache2

ADD sites/*.conf /etc/apache2/sites-available/
RUN ln -s /etc/apache2/sites-available/*.conf /etc/apache2/sites-enabled/

EXPOSE 80

WORKDIR /var/www

ENTRYPOINT ["/opt/docker/bin/entrypoint.sh"]

CMD ["supervisord"]