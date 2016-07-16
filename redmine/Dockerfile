# Fork from serverbee/redmine-alpine
FROM alpine
MAINTAINER Shengjing Zhu <zsj950618@gmail.com>

ENV REDMINE_VERSION=3.3.0 \
    GOSU_VERSION=1.9

ENV REDMINE_TAR="https://github.com/redmine/redmine/archive/$REDMINE_VERSION.tar.gz" \
    THEME_TAR="https://github.com/hardpixel/minelab/archive/master.tar.gz" \
    GOSU_BIN="https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-amd64" \
    RAILS_ENV=production \
    BUNDLE_SILENCE_ROOT_WARNING=1

RUN sh -x \
    && cd / \
    && apk --update add --virtual .redmine-rundeps \
         ruby ruby-bundler ruby-bigdecimal git imagemagick sqlite-libs tzdata \
    && apk add --virtual .redmine-builddpes \
         curl build-base ruby-dev imagemagick-dev sqlite-dev zlib-dev \
    && curl -sSL $GOSU_BIN -o /usr/local/bin/gosu \
    && chmod +x /usr/local/bin/gosu \
    && curl -sSL $REDMINE_TAR | tar xz \
    && mv redmine-$REDMINE_VERSION redmine \
    && cd /redmine \
    && rm files/delete.me log/delete.me \
    && echo "$RAILS_ENV:" > config/database.yml \
    && echo "  adapter: sqlite3" >> config/database.yml \
    && echo "  database: files/redmine.sqlite3" >> config/database.yml \
    && bundle install --without development test \
    && cd public/themes/ \
    && curl -sSL $THEME_TAR | tar xz \
    && mv minelab-master minelab \
    && rm -rf ~/.bundle/ \
    && apk --purge del .redmine-builddpes \
    && rm -rf /var/cache/apk/* \
    && adduser -h ${PWD} -D -H -s /sbin/nologin redmine \
    && chown -R redmine:redmine /redmine

WORKDIR /redmine

VOLUME ["/redmine/files"]

COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 3000

CMD ["rails", "server", "-b", "0.0.0.0"]