FROM laradock/workspace:1.8-71

MAINTAINER Kunal Mangaraj <mangaraj.kunal@gmail.com>

USER root

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN groupadd -g ${PGID} vagrant && \
    useradd -u ${PUID} -g vagrant -m vagrant -G docker_env && \
    usermod -p "*" vagrant && \
    apt-get update -yqq

USER root

RUN add-apt-repository -y ppa:ondrej/php && \
  apt-get update -yqq && \
  apt-get -y install libxml2-dev php7.1-soap

ARG TZ=UTC
ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

COPY ./composer.json /home/vagrant/.composer/composer.json

RUN chown -R vagrant:vagrant /home/vagrant/.composer

USER vagrant

RUN composer global install

USER root

COPY ./crontab /etc/cron.d
RUN chmod -R 644 /etc/cron.d

USER vagrant
COPY ./aliases.sh /home/vagrant/aliases.sh
RUN echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source /home/vagrant/aliases.sh" >> ~/.bashrc && \
    echo "" >> ~/.bashrc

USER root
RUN echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source /home/vagrant/aliases.sh" >> ~/.bashrc && \
    echo "" >> ~/.bashrc

RUN apt-get install -y --force-yes php-xdebug && \
    sed -i 's/^;//g' /etc/php/7.1/cli/conf.d/20-xdebug.ini && \
    echo "alias phpunit='php -dzend_extension=xdebug.so /var/www/vendor/bin/phpunit'" >> ~/.bashrc

COPY ./xdebug.ini /etc/php/7.1/cli/conf.d/xdebug.ini

USER vagrant

ARG NODE_VERSION=stable
ENV NODE_VERSION ${NODE_VERSION}
ARG INSTALL_NODE=false
ENV INSTALL_NODE ${INSTALL_NODE}
ENV NVM_DIR /home/vagrant/.nvm
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash && \
        . $NVM_DIR/nvm.sh && \
        nvm install 8.9.4 && \
        nvm use 8.9.4 && \
        nvm alias default 8.9.4 && \
        npm install -g gulp bower vue-cli

# Wouldn't execute when added to the RUN statement in the above block
# Source NVM when loading bash since ~/.profile isn't loaded on non-login shell
RUN echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc

# Add NVM binaries to root's .bashrc
USER root

RUN echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="/home/vagrant/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc

USER vagrant

ARG YARN_VERSION=latest
ENV YARN_VERSION ${YARN_VERSION}

RUN   [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" && \
    if [ ${YARN_VERSION} = "latest" ]; then \
        curl -o- -L https://yarnpkg.com/install.sh | bash; \
    else \
        curl -o- -L https://yarnpkg.com/install.sh | bash -s -- --version ${YARN_VERSION}; \
    fi && \
    echo "" >> ~/.bashrc && \
    echo 'export PATH="$HOME/.yarn/bin:$PATH"' >> ~/.bashrc

# Add YARN binaries to root's .bashrc
USER root

RUN echo "" >> ~/.bashrc && \
    echo 'export YARN_DIR="/home/vagrant/.yarn"' >> ~/.bashrc && \
    echo 'export PATH="$YARN_DIR/bin:$PATH"' >> ~/.bashrc

USER vagrant

RUN echo "" >> ~/.bashrc && \
    echo 'export PATH="/var/www/vendor/bin:$PATH"' >> ~/.bashrc

USER root
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www
