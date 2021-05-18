# TestTask

В этом репозитории содержится код тестового задания на стажировку в "Экспресс 42".

Задание состоит из двух частей:

* [Установка Redmine + PostgreSQL в Docker-образе](./Part1)
* [Разбиение установки на три контейнера с помощью docker-compose](./Part2)

Также настроен [CI/CD pipeline](https://travis-ci.com/github/Samofimp/TestTask) для отправки образа в [Docker Hub](https://hub.docker.com/repository/docker/samofimp/redmine).

## Создание образа, запуск контейнера

```
docker build -t redmine Part1
docker run -p 8080:8080 redmine
```

## Запуск docker-compose

```
cd Part2
docker-compose up --build
```

## Загрузка образа в Docker Hub

```
echo "$DOCKERHUB_PASSWORD" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin
docker tag redmine samofimp/redmine
docker push samofimp/redmine
```

>`DOCKERHUB_USERNAME` - имя пользователя Docker Hub
>
>`DOCKERHUB_PASSWORD` - авторизационный токен