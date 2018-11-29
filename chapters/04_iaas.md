@title[IaaS]
## iaas

```bash
PHP_VERSION=7.2
echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu xenial main" > /etc/apt/sources.list.d/ondrej-php.list && \
echo "deb http://ppa.launchpad.net/ondrej/php-qa/ubuntu xenial main" > /etc/apt/sources.list.d/ondrej-php-qa.list && \
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4F4EA0AAE5267A6C && \
apt-get update && apt-get -y --no-install-recommends install libgeos-dev \
  php$PHP_VERSION-fpm \
  php$PHP_VERSION-mysql \
  php$PHP_VERSION-curl \
  php$PHP_VERSION-gd \
  php$PHP_VERSION-mbstring \
  php$PHP_VERSION-imap \
  php$PHP_VERSION-zip \
  php$PHP_VERSION-xml

sed -i 's/^;cgi.fix_pathinfo=.*$/cgi.fix_pathinfo=0/' /etc/php/$PHP_VERSION/fpm/php.ini
sed -i 's/^memory_limit =.*$/memory_limit = 512M/' /etc/php/$PHP_VERSION/fpm/php.ini
sed -i 's/^upload_max_filesize =.*$/upload_max_filesize = 128M/' /etc/php/$PHP_VERSION/fpm/php.ini
sed -i 's/^post_max_size =.*$/post_max_size = 128M/' /etc/php/$PHP_VERSION/fpm/php.ini

sed -i 's/^;clear_env = no/clear_env = no/' /etc/php/$PHP_VERSION/fpm/pool.d/www.conf

curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

```