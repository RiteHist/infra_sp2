# API YaMDb 
## Описание
YaMDb - это новый веб сервис, который собирает отзывы пользователей на различные произведения (книги, фильмы, музыку и т.д.) Через этот интерфейс могут работать мобильное приложение или чат-бот; через него же можно будет передавать данные в любое приложение или на фронтенд.

#### Реализован функционал, дающий возможность:
* Оставлять и редактировать отзывы на произведения.
* Комментировать отзывы и редактировать эти комментарии.
* Добавлять в базу данных новые произведения, категории, жанры (для прользователей с правами доступа Администратора) 
* Ставить оценки произведениям, видеть их рейтинг.

Для выполнения всех запросов существуют различные уровни доступа (автор поста/комментария, авторизованный пользователь, неавторизованный пользователь, модератор, администратор, суперюзер Django).

Механизм авторизации пользователей реализован с помощью JWT-токенов, причем токен пользователь получает после подтверждения своего e-mail 

## Стек технологий: 

### python 3.7
### django_rest_framwork
### Django ORM
### REST API
### PostgreSQL
### Simple-JWT
### Docker


Более подробная документация и описание функции проекта доступны по адресу 
http://localhost/redoс


## Как запустить проект на локальной машине:
### Для Windows:
Необходимо скачать Git bash и Docker и установить их:

https://gitforwindows.org/

https://www.docker.com/get-started/

Запустите Git bash

### Если у вас Linux или macOS:

Запустите программу Терминал

### Далее описывается алгоритм для любой из вышеперечисленных ОС:

Создание рабочей папки:

```
mkdir <<название проекта, папки>>
```

Либо перейти в нужную папку:

```
cd <<название папки>>
```

Клонировать репозиторий:

```
git clone git@github.com:RiteHist/infra_sp2.git
```

Перейти в папку infra:

```
cd infra
```

Создайте файл .env и заполните его соглано примеру из файла .env.example:

```
touch .env
```

Запустить контейнеры через Docker:

```
docker-compose up -d --build
```

Выполните миграции:

```
docker-compose exec web python manage.py migrate
```

Загрузите статику:

```
docker-compose exec web python manage.py collectstatic
```

Для заполнения базы данными используйте следующую команду:

```
docker-compose exec web python manage.py loaddata <<имя файла с данными.json>>
```


### Получаем JWT-токен
На эндпоинт http://localhost/api/v1/auth/signup/ передаем POST запрос с параметрами username и email. 
В ответ на указанный email получаем confirmation_code.
Далее, на эндпоинт http://localhost/api/v1/auth/token/ передаем POST запрос с параметрами username и confirmation_code. В ответ получаем JWT-токен, который используем для выполнения запросов POST, DELETE, PATCH, PUT как авторизованный пользователь.

При отправке запроса передавайте токен в заголовке Authorization: Bearer <<токен>>

### Создаем новoe произведение 
Передаем POST-запрос на адрес http://localhost/api/v1/titles/ 
Обязательные поля:   
```
{ 
    "name": "Красная шапочка",  
    "year": 1697,  
    "description": "Старинное произведение Ш.Перро",  
    "genre": [  
    "сказка"  
    ],  
    "category": "книга"  
}  
```
Ответ будет выглядеть следующим образом:   

```
{  
    "id": 1,  
    "name": "Красная шапочка",  
    "year": 1697,  
    "rating": 0,  
    "description": "Старинное произведение Ш.Перро",  
    "genre": [  
        "name": "сказка",  
        "slug": "tale"  
    ],  
    "category": {  
        "name": "книга",  
        "slug": "book"  
    } 
} 
```

### Получаем отзыв по id для указанного произведения.
Передаем GET-запрос на адрес http://localhost/api/v1/titles/{title_id}/reviews/{review_id}/

Ответ будет выглядеть следующим образом:

```
{  
    "id": review_id,  
    "text": "string",  
    "author": "string",  
    "score": 1,  
    "pub_date": "2019-08-24T14:15:22Z"   
} 
```

### Частично обновляем комментарий к отзыву по id.
Для этого действия вы должны быть автором комментария, модератором или администратором. 
Передаем PATCH запрос на эндпоинт http://localhost/api/v1/titles/{title_id}/reviews/{review_id}/comments/{comment_id}/ с параметром "text": "Измененный текст комментария". 

Ответ будет выглядеть следующим образом:  

```
{ 
    "id": title_id,  
    "text": "Измененный текст комментария",  
    "author": "string",  
    "pub_date": "2019-08-24T14:15:22Z"  
}   
```
### Добавляем новый жанр
Для этого действия необходимо обладать правами администратора.  
Передаем POST запрос на эндпоинт http://localhost/api/v1/genres/  
Заполняем поля:

```
{
  "name": "ужасы",
  "slug": "horror"
}
```
Ответ будет выглядеть следующим образом:

```
{
  "name": "ужасы",
  "slug": "horror"
}
```




## Контакты
Широков Анатолий derpy.hooves.ru@gmail.com
