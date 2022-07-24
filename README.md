# parsing_infrastructure
Parsing infrastructure with Django, Celery, Redis and Docker.

Проект по созданю инфраструктуры для парсинга на основе Django, Celery, Redis и Docker.
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

Запускаем приложение

docker-compose up --build -d

Далее зайдем в докер-контейнер и создадим суперпользователя

docker ps # intended to find django container name --> code_django_1
>> 6f5db39cfa3b   code_django                   "sh -c ' python mana…"   47 seconds ago   Up 46 seconds                   8000/tcp                 code_django_1

docker exec -ti code_django_1 bash # go into the container
python manage.py createsuperuser # create admin user in django
python manage.py collectstatic # intended to load css and js files
exit


Результат
5 Использование приложения
5.1 Запрос парсинга POST

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
5.2 Посмотрим результат задачи

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

### Автор
Валентин
