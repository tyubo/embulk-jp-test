FROM java:8-jre

RUN apt-get update && \
    apt-get -y install openssh-client

RUN curl -o /bin/embulk -L "https://dl.bintray.com/embulk/maven/embulk-0.9.5.jar" && \
    chmod +x /bin/embulk

RUN mkdir -p /var/lib/embulk
WORKDIR /var/lib/embulk

COPY Gemfile .
COPY Gemfile.lock .
COPY jp_test.yml.liquid .
COPY _query.yml.liquid .

RUN embulk bundle install --path vendor/bundle
