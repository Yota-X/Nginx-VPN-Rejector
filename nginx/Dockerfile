FROM alpine:3.16

RUN apk --update --no-cache add \
    build-base \
    linux-headers \
    libaio \
    libressl-dev \
    cmake \
    git \
    libtool \
    autoconf \
    automake \
    libmaxminddb-dev \
    pcre-dev \
    zlib-dev \
    intltool \
    gettext \
    gzip \
    gcc \
    wget

WORKDIR /opt

RUN git clone https://github.com/ip2location/ip2proxy-c.git \
    && cd /opt/ip2proxy-c \
    && autoreconf -i -v --force

RUN cd /opt/ip2proxy-c \
    && ./configure \
    && make \
    && make install

RUN git clone https://github.com/ip2location/ip2proxy-nginx.git \
    && git clone https://github.com/leev/ngx_http_geoip2_module.git --depth=1 \
    && wget https://nginx.org/download/nginx-1.22.0.tar.gz \
    && tar xvfz nginx-1.22.0.tar.gz

RUN cd /opt/nginx-1.22.0 \
    && ./configure --with-http_ssl_module \
    --conf-path=/etc/nginx/nginx.conf \
    --pid-path=/var/run/nginx.pid \
    --sbin-path=/usr/local/sbin/nginx \
    --with-http_stub_status_module \
    --with-http_gzip_static_module \
    --with-http_secure_link_module \
    --with-http_realip_module \
    --with-threads \
    --with-file-aio \
    --add-dynamic-module=/opt/ip2proxy-nginx \
    --add-dynamic-module=/opt/ngx_http_geoip2_module \
    && make \
    && make install

# Create user
RUN set -x ; \
    addgroup -g 1003 -S metanit ; \
    adduser -u 1001 -D -S -G metanit metanit && exit 0 ; exit 1

RUN apk del \
    git \
    wget

WORKDIR /root

EXPOSE 80 443

ENTRYPOINT ["/usr/local/nginx/sbin/nginx", "-g", "daemon off;"]