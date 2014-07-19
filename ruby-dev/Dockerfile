FROM ubuntu:precise
MAINTAINER Raf Szalansky github.com/szalansky

RUN apt-get update
RUN apt-get install -qy unattended-upgrades curl ca-certificates git-core \
  build-essential autoconf libssl-dev libreadline6-dev libpq-dev \
  libqt4-dev libqtwebkit-dev nodejs

# Install ruby-build & configure
RUN git clone https://github.com/sstephenson/ruby-build.git /tmp/ruby-build
RUN /tmp/ruby-build/install.sh
RUN rm -r /tmp/ruby-build
ENV CONFIGURE_OPTS --disable-install-doc
RUN echo 'gem: --no-rdoc --no-ri' >> ~/.gemrc

# Install specified Ruby version
RUN echo '2.1.2' > /tmp/.ruby-version
RUN ruby-build `cat /tmp/.ruby-version` /usr/local
RUN gem install bundler

# Create application user
RUN useradd -m -s /bin/bash app
ONBUILD WORKDIR /home/app

# Install dependencies
ONBUILD ADD Gemfile /home/app/
ONBUILD ADD Gemfile.lock /home/app/
ONBUILD RUN bundle install

# Add application
ONBUILD ADD . /home/app/
ONBUILD RUN chown -R app:app /home/app
ONBUILD USER app
ONBUILD ENV HOME /home/app
