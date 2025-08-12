FROM alpine:3

VOLUME ["/var/svn", "/var/www/localhost/htdocs/svnadmin/data", "/etc/apache2/svn-auth"]

ARG SERVER_ADMIN_EMAIL

RUN apk add --no-cache \
      apache2 apache2-utils apache2-webdav mod_dav_svn \
      subversion \
      php84 php84-apache2 php84-json php84-session php84-ldap php84-xml \
      wget \
  && sed -i "s/;extension=ldap/extension=ldap/" /etc/php84/php.ini \
  && sed -i "s/^Listen 80$/Listen 8080/" /etc/apache2/httpd.conf \
  && sed -i "s/^#\?ServerAdmin.*$/ServerAdmin ${SERVER_ADMIN_EMAIL}/" /etc/apache2/httpd.conf \
  && sed -i '/^[[:space:]]*ErrorLog/d' /etc/apache2/httpd.conf \
  && sed -i '/^[[:space:]]*CustomLog/d' /etc/apache2/httpd.conf \
  && mkdir -p /var/svn /run/apache2 /etc/apache2/svn-auth

COPY dav_svn.authz dav_svn.passwd /etc/apache2/svn-auth/

COPY zz-dav-svn.conf zz-logging.conf /etc/apache2/conf.d/

RUN wget -qO /tmp/svnadmin.tar.gz https://github.com/Th120/iF.SVNAdmin/archive/refs/tags/v1.7.0-01.03.25-unofficial.tar.gz \
  && mkdir -p /var/www/localhost/htdocs/svnadmin/data  \
  && tar -xzf /tmp/svnadmin.tar.gz -C /var/www/localhost/htdocs/svnadmin --strip-components=1 \
  && rm -f /tmp/svnadmin.tar.gz 

RUN chown -R apache:apache /var/svn /etc/apache2 /run/apache2 /var/www/localhost/htdocs \
  && chmod -R 777 /var/www/localhost/htdocs/svnadmin/data \
  && chmod 660 /etc/apache2/svn-auth/dav_svn.authz /etc/apache2/svn-auth/dav_svn.passwd

EXPOSE 8080
USER apache
CMD ["httpd", "-D", "FOREGROUND"]
