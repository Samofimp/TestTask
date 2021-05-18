# Ubuntu используется в качестве базового образа
FROM ubuntu

# Обновление списка репозиториев
RUN apt update

# Установка Ruby и необходимых библиотек
RUN apt install build-essential ruby-dev -y
RUN gem install bundler

# Установка Redmine
# libpq-dev нужен для работы pg гема
RUN apt install git libpq-dev -y
RUN git clone https://github.com/redmine/redmine
WORKDIR /redmine

# Файл с конфигурацией для коннекта к базе
COPY redmine-config.yml config/database.yml

# Установка зависимостей
COPY Gemfile.local Gemfile.local
RUN bundle install --without development test
RUN bundle exec rake generate_secret_token
ENV RAILS_ENV=production

# Запуск Redmine
CMD rm -f /redmine/tmp/pids/server.pid && bundle exec rake db:migrate && rails server -p 8080