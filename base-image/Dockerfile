FROM ubuntu:20.04

# set noninteractive installation
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=UTC
RUN apt update -y && \
    apt --no-install-recommends -y \
        install ruby2.7 libruby2.7 ruby2.7-dev \
        libxml2 \
        build-essential \
        libssl-dev \
        libmysqlclient-dev \
        git \
        wget \
        nodejs \
        libcap2-bin 

RUN mkdir -p /opt/postal/app /opt/postal/config

RUN setcap 'cap_net_bind_service=+ep' /usr/bin/ruby2.7

RUN gem install bundler procodile

RUN wget https://postal.atech.media/packages/stable/latest.tgz -O - | tar zxpv -C /opt/postal/app

# RUN ln -s /opt/postal/app/bin/postal /usr/bin/postal

WORKDIR /opt/postal/app

# cannot use postal bundle <path> since bundler in Gemfile.lock is out of date
# https://github.com/postalhq/postal/blob/master/bin/postal
#RUN postal bundle /opt/postal/vendor/bundle
#RUN bundle update --bundle --path=/opt/postal/vendor/bundle --jobs=2 --clean --without=development assets
RUN bundle update --bundler --jobs=4 && rm -rf vendor
RUN bundler config path /opt/postal/vendor
RUN bundle install --path=/opt/postal/vendor/bundle --jobs=2 --clean --without=development assets
