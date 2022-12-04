# Проект API YAMDB
### Описание проекта:
В проекте Yamdb можно оставить отзыв на произведения ("Книги", "Фильмы", "Музыка" и пр.). Отзывы состоят из текстовой заметки и оценки от 1 до 10. Рейтинг складывается из оценок. Произведения можно отфильтровать по категориям, либо по жанру.

![Workflow status](https://github.com/AlexanderAvrov/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

### Шаблон заполнения env-файла
- DB_ENGINE=django.db.backends.postgresql `указываем, что работаем с postgresql`
- DB_NAME=postgres `имя базы данных`
- POSTGRES_USER=postgres `логин для подключения к базе данных`
- POSTGRES_PASSWORD=postgres `пароль для подключения к БД (установите свой)`
- DB_HOST=db  `название сервиса (контейнера)`
- DB_PORT=5432 `порт для подключения к БД`

### Запуск приложения используя контейнеры
1. Перейти в папку infa: ```cd infra```
2. Собрать контейнеры: ```docker-compose up -d --build```
3. Применить миграции: ```docker-compose exec web python manage.py migrate```
4. Cобрать статику: ```docker-compose exec web python manage.py collectstatic --no-input```

### Загрузить базу данных в приложение
```sh
docker-compose exec web python manage.py loaddata fixtures.json
```
### Остановка работы контейнеров
```sh
docker-compose stop
```

### Документация по запросам
см. yatube_api/static/redoc.yaml  или после запуска на localhost по ссылке http://localhost/redoc/

### Примеры запросов API к сайту
Запрос на просмотр произведений:
```
GET http://127.0.0.1:8000/api/v1/titles/
```
Ответ: 
```sh
{    
    "count": 1,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 1,
            "name": "Побег из Шоушенка",
            "year": 1994,
            "rating": 9,
            "description": "Фильм про побег из тюрьмы",
            "genre": [
                {
                    "name": "Драма",
                    "slug": "drama"
                }
            ],
            "category": {
                "name": "Фильм",
                "slug": "movie"
            }
        }
    ]
}
```
Запрос на получение жанров:
```
GET http://127.0.0.1:8000/api/v1/genres/
```
Ответ: 
```sh
{
    "count": 3,
    "next": null,
    "previous": null,
    "results": [
        {
            "name": "Драма",
            "slug": "drama"
        },
        {
            "name": "Комедия",
            "slug": "comedy"
        },
        {
            "name": "Вестерн",
            "slug": "western"
        }
    ]
}
```
Запрос на получение категорий:
```
GET http://127.0.0.1:8000/api/v1/categories/
```
Ответ: 
```sh
{
    "count": 3,
    "next": null,
    "previous": null,
    "results": [
        {
            "name": "Фильм",
            "slug": "movie"
        },
        {
            "name": "Книга",
            "slug": "book"
        },
        {
            "name": "Музыка",
            "slug": "music"
        }
    ]
}
```
Запрос на получение отзывов к произведению:
```
GET http://127.0.0.1:8000/api/v1/titles/1/reviews/
```
Ответ: 
```sh
{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 1,
            "text": "Мне понравилось",
            "author": "bingobongo",
            "score": 9,
            "pub_date": "2019-09-24T21:08:21.567000Z"
        },
        {
            "id": 2,
            "text": "Эти стены имеют одно свойство: сначала ты их ненавидишь, потом привыкаешь, а потом не можешь без них жить",
            "author": "capt_obvious",
            "score": 10,
            "pub_date": "2019-09-24T21:08:21.567000Z"
        }
    ]
}
```
Запрос на получение комментариев к отзыву:
```
GET http://127.0.0.1:8000/api/v1/titles/1/reviews/1/comments/
```
Ответ: 
```sh
{
    "count": 1,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 5,
            "text": "Солидарен",
            "author": "anonymous",
            "pub_date": "2022-09-10T19:18:38.659928Z"
        }
    ]
}
```
Запрос регистрации нового пользователя:
```
POST http://127.0.0.1:8000/api/v1/auth/signup/
```
```sh
{    
    "username": "Alex",
    "email": "myemail@mail.com"
}
```
Ответ:
```sh
{    
    "username": "Alex",
    "email": "myemail@mail.com"
}
```
Запрос на получение токена:
```
POST http://127.0.0.1:8000/api/v1/auth/token/
```
```sh
{
    "username": "Alex",
    "confirmation_code": "642-5e3b203c6ac94a6cd822"
}
```
Ответ:
```sh
{    
    "token": "efyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjYzMjY5MDcwLCJpYXdvdsQiOjE2NjI4MzcwNzAsImp0aSI6IjM4M2U4MGI1NWZhODQ3ZGM4MWE2ZTM3MGI2NjdjYzBjIiwidXNlcl9pZCI6MTA2fQ.3JQxxkFOIjdISs7FgtfArdJhQ32JyIsEj6N5phzYqf0"
}
```
