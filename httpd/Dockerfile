FROM httpd:2.4.46-alpine
RUN apk update; \
    apk upgrade;
    
# Copy apache vhost file to proxy php requests to php-fpm container
COPY *.conf /usr/local/apache2/conf/
RUN echo "Include /usr/local/apache2/conf/apache.conf" \
    >> /usr/local/apache2/conf/httpd.conf

# Copy cert/key for SSL
COPY certs/* /usr/local/apache2/conf/
# Activate SSL config, if cert exists
RUN if [ -f "/usr/local/apache2/conf/cert.pem" ] ; then \
    echo "Include /usr/local/apache2/conf/ssl.conf" \
    >> /usr/local/apache2/conf/httpd.conf; \
    fi;