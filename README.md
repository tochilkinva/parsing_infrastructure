# parsing_infrastructure
Parsing infrastructure with Django, Celery, Redis and Docker.

Проект по созданию инфраструктуры для парсинга на основе Django, Celery, Redis и Docker.

По материалам статьи https://habr.com/ru/post/662928/

Сайт для парсинга https://books.toscrape.com/

### Технологии
Python 3.9, Django 4, Celery, Redis, PostgreSQL

### Как запустить проект:

Клонировать репозиторий и перейти в него в командной строке:

```
git clone https://github.com/tochilkinva/parsing_infrastructure.git
cd parsing_infrastructure
```

Cоздать и активировать виртуальное окружение:

```
python -m venv venv
. env/Scripts/activate
```

Установить зависимости из файла requirements.txt:

```
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Создать файл .env в корне, пример есть в .env_example

Запускаем сборку контейнера всего приложения

```
docker-compose up --build -d
```

Далее зайдем в докер-контейнер и создадим суперпользователя. Нам нужен parsing_infrastructure-django-1

```
docker ps # список запущенных контейнеров
docker exec -ti parsing_infrastructure-django-1 bash # заходим в контейнер с django
python manage.py createsuperuser # создаем админа
python manage.py collectstatic # собираем статику
exit # выходим из контейнера
```

### Проверка работы


Запрос POST для создания задачи парсинга (запомните task_id или смотрите в админке)

```
# POST http://127.0.0.1:80/task
{
    "type": "philosophy_7"
}

RESPONSE:
{
    "message": "Create task",
    "task_id": "062ac81f-dafe-4e2c-95e9-c042936e85f3",
    "data": {
        "type": "philosophy_7"
    }
}
```

Запрос GET для просмотра результата задачи парсинга

```
# GET http://127.0.0.1:80/task
{
    "task_id": "062ac81f-dafe-4e2c-95e9-c042936e85f3"
}

RESPONSE:
{
    "task_id": "062ac81f-dafe-4e2c-95e9-c042936e85f3",
    "task_status": "SUCCESS",
    "task_result": true
}
```

### Автор
Валентин
