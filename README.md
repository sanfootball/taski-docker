# taski-docker

### Домен: https://mykittyproject.ddns.net/

## Оглавление:
- [Стек технологий.](#Стек-технологий)
- [Краткое описание проекта.](#Краткое-описание-проекта)
- [Как запустить проект.](#Как-запустить-проект)

## Стек технологий
- Python
- Django
- DRF
- Docker
- Nginx
- Gunicorn
- GitHub Actions
- Linux

## Краткое описание проекта:
Проект был размещен на сервере Ubuntu в контейнерах при помощи Docker, есть 3 контейнера:
- nginx
- backend
- frontend
  
В файле docker-compose описывается система из контейнеров. Docker-compose-production описывает уже непосредственно вариант с работой через GitHub Actions и деплоем на сервере.

Реализован деплой на сервере после загрузки на GitHub при помощи GitHub Actions. Перед деплоем GitHub Actions делает следующее:
- тестирует backend код на соответствие PEP8
- тестирует frontend код
- загружает на DockerHub все контейнеры (c nginx, backend и frontend)
- с DockerHub осуществляет деплой на сервер
- направлят в Телеграме сообщение о совершенном деплое, уточняя:
  - кто сделал коммит
  - какое сообщение было у коммита
  - ссылку на коммит

## Как запустить проект:
1. Скачать docker на сервер, если его нет. Инструкции: https://docs.docker.com/get-docker/

2. Клонировать репозиторий с проектом на свой компьютер:
   ```git clone git@github.com:gaifut/taski-docker.git```

3. Установить и активировать виртуальное окружение: 
```
python -m venv venv
. venv/Script/activate
```
В виртуальном окружении установить зависимости:
```
pip install -r requirements.txt
```

4. Создать .env файл со сделующей информацией:                                                       
POSTGRES_USER= логин
POSTGRES_PASSWORD= пароль
POSTGRES_DB= имя БД
DB_HOST= название хоста
DB_PORT=5432

5. Выполнить сборку контейнеров: ```docker-compose up -d --build```
6. Выполнить миграции: ```docker-compose exec backend python manage.py migrate```
7. Создайть суперпользователя: ```docker-compose exec backend python manage.py createsuperuser```
8. Собрать файлы статики: ```docker-compose exec backend python manage.py collectstatic```
9. Скопируйть файлы статики в /backend_static/static/ backend-контейнера: ```docker compose exec backend cp -r /app/collected_static/. /backend_static/static/```

- 127.0.0.1:8000 Главная страница

