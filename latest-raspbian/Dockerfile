FROM balenalib/raspberrypi3-debian:stretch-run-20190327
USER root

RUN [ "cross-build-start" ]
RUN mkdir /source
COPY latest-raspbian/config/* /source/
RUN apt-get update && apt-get install -y git && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y python-pip python-setuptools rtorrent unzip unrar-free mediainfo curl php-fpm php-cli php-geoip php-mbstring nginx wget ffmpeg supervisor php-xml libarchive-zip-perl libjson-perl libxml-libxml-perl sox && \
    #update-alternatives --install /usr/bin/python python /usr/bin/python3 2 && \
    #rm -f /usr/bin/python && ln -s /usr/bin/python3 /usr/bin/python && \
    rm -rf /var/lib/apt/lists/* && \
    pip install --no-cache-dir cfscrape && \
    pip install --no-cache-dir --upgrade setuptools && \
    #rm -rf /var/cache/* && \
    cp /source/rutorrent-*.nginx /root/ && \
    mkdir -p /var/www && cd /var/www && \
    git clone https://github.com/Novik/ruTorrent.git rutorrent && \
    rm -rf ./rutorrent/.git* && cd / && \
    cp /source/config.php /var/www/rutorrent/conf/ && \
    cp /source/startup-rtorrent.sh /source/startup-nginx.sh /source/startup-php.sh /source/.rtorrent.rc /root/ && \
    chmod +x /root/* && \
    cp /source/supervisord.conf /etc/supervisor/conf.d/ && \
    sed -i 's/\/var\/log/\/downloads\/\.log/g' /etc/nginx/nginx.conf && \
    rm -rf /source
    
 
RUN [ "cross-build-end" ]

EXPOSE 80 443 49160 49161 5000
VOLUME /downloads
 
CMD ["supervisord"]
