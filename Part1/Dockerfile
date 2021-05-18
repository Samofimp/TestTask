# Ubuntu используется в качестве базового образа
FROM ubuntu

# Обновление списка репозиториев
RUN apt update

# Установка Ruby и необходимых библиотек
RUN apt install build-essential ruby-dev -y
RUN gem install bundler

# Установка PostgreSQL
# DEBIAN_FRONTEND=noninteractive, чтобы пакет tzdata не требовал настроек
ENV DEBIAN_FRONTEND noninteractive
RUN apt install postgresql -y

# Настройка БД по инструкции Redmine
COPY postgres-config.sql /
RUN chmod +x postgres-config.sql
# Создание пользователя и БД для работы Redmine
RUN service postgresql start && su postgres -c "psql -a -f /postgres-config.sql"

# Установка Redmine
# libpq-dev нужен для работы pg гема
RUN apt install git libpq-dev -y
RUN git clone https://github.com/redmine/redmine
WORKDIR /redmine

# Файл с конфигурацией для коннекта к базе
COPY redmine-config.yml config/database.yml

# Установка зависимостей
RUN bundle install --without development test
RUN bundle exec rake generate_secret_token
ENV RAILS_ENV=production

# Запуск Redmine
# PostgreSQL не успевает запуститься за таймаут, нужно чуть больше времени
CMD (service postgresql start || true) && sleep 10 && bundle exec rake db:migrate && rails server -p 8080