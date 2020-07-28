FROM php:7-fpm-alpine

# This Dockerfile adds a non-root user with sudo access. Update the “remoteUser” property in
# devcontainer.json to use it. More info: https://aka.ms/vscode-remote/containers/non-root-user.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create a non-root user to use if preferred - see https://aka.ms/vscode-remote/containers/non-root-user.
RUN addgroup --gid $USER_GID $USERNAME \
    && adduser --system --shell /bin/bash --uid $USER_UID --ingroup $USERNAME $USERNAME

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    && composer global require hirak/prestissimo \
    && composer clear-cache

# Install extensions
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync \
    && install-php-extensions gd xdebug \
    && rm /usr/local/bin/install-php-extensions \
    #
    # Configure xdebug
    && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini
    
# Configure PHP
RUN echo "user = ${USERNAME}" >> /usr/local/etc/php-fpm.d/www.conf \
    && echo "group = ${USERNAME}" >> /usr/local/etc/php-fpm.d/www.conf