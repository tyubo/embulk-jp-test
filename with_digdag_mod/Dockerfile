FROM java:8-jre

RUN apt-get update && \
    apt-get -y install openssh-client

RUN sed -i '/jessie-updates/d' /etc/apt/sources.list &&\
    rm -f /etc/apt/sources.list.d/jessie-backports.list &&\
    cat /etc/apt/sources.list &&\
    apt-get update -qq &&\
    apt-get install -y locales

RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
ENV LANG="ja_JP.UTF-8" \
    LANGUAGE="ja_JP:ja" \
    LC_ALL="ja_JP.UTF-8"

RUN curl -o /bin/embulk -L "https://dl.bintray.com/embulk/maven/embulk-0.9.5.jar" && \
    chmod +x /bin/embulk

RUN mkdir -p /var/lib/embulk
WORKDIR /var/lib/embulk

COPY Gemfile .
COPY Gemfile.lock .

RUN embulk bundle install --path vendor/bundle
