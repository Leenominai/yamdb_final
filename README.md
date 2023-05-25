# Проект YaMDb собирает отзывы пользователей на произведения.

https://github.com/leenominai/yamdb_final/actions/workflows/yamdb_workflow/badge.svg

## Описание:
Проект YaMDb собирает отзывы пользователей на произведения. Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
Произведения делятся на категории, такие как «Книги», «Фильмы», «Музыка». Например, в категории «Книги» могут быть произведения «Винни-Пух и все-все-все» и «Марсианские хроники», а в категории «Музыка» — песня «Давеча» группы «Жуки» и вторая сюита Баха. Список категорий может быть расширен (например, можно добавить категорию «Изобразительное искусство» или «Ювелирка»).

Произведению может быть присвоен жанр из списка предустановленных (например, «Сказка», «Рок» или «Артхаус»).

Добавлять произведения, категории и жанры может только администратор.

Благодарные или возмущённые пользователи оставляют к произведениям текстовые отзывы и ставят произведению оценку в диапазоне от одного до десяти (целое число); из пользовательских оценок формируется усреднённая оценка произведения — рейтинг (целое число). На одно произведение пользователь может оставить только один отзыв.
Пользователи могут оставлять комментарии к отзывам.

Добавлять отзывы, комментарии и ставить оценки могут только аутентифицированные пользователи.

## Регистрация пользователей:
1. Пользователь отправляет POST-запрос с параметрами username и email на эндпоинт /api/v1/auth/signup/.
2. YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный email адрес.
3. Пользователь отправляет POST-запрос с параметрами email и confirmation_code на эндпоинт /api/v1/auth/token/,
после чего пользователь получает JWT token. Для авторизации вы должны передавать ключ в заголовке: \
key - "Authorization", value "Bearer и ваше значение token" 

### Пользователя может создавать администратор.
1. Администратор отправляет POST-запрос с параметрами username и email на эндпоинт /api/v1/users/. \
При создании пользователя не предполагается автоматическая отправка письма пользователю с кодом подтверждения.
2. После этого пользователь должен самостоятельно отправить свой email и username на эндпоинт /api/v1/auth/signup/.
3. YaMDB отправляет письмо с кодом подтверждения (confirmation_code) на указанный email адрес.
4. Далее пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит JWT-токен, как и при самостоятельной регистрации.


## Пользовательские роли и права доступа.
- Аноним — может просматривать описания произведений, читать отзывы и комментарии.
- Аутентифицированный пользователь (user) — может читать всё, как и Аноним, может публиковать отзывы и ставить оценки произведениям (фильмам/книгам/песенкам), может комментировать отзывы; может редактировать и удалять свои отзывы и комментарии, редактировать свои оценки произведений. Эта роль присваивается по умолчанию каждому новому пользователю.
- Модератор (moderator) — те же права, что и у Аутентифицированного пользователя, плюс право удалять и редактировать любые отзывы и комментарии.
- Администратор (admin) — полные права на управление всем контентом проекта. Может создавать и удалять произведения, категории и жанры. Может назначать роли пользователям.
- Суперюзер Django обладает правами администратора, пользователя с правами admin. 

## Ресурсы API YaMDb
* Ресурс auth: аутентификация.
* Ресурс users: пользователи.
* Ресурс titles: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).
* Ресурс categories: категории (типы) произведений («Фильмы», «Книги», «Музыка»). Одно произведение может быть привязано только к одной категории.
* Ресурс genres: жанры произведений.
* Ресурс reviews: отзывы на произведения.
* Ресурс comments: комментарии к отзывам.

## Технологии:
Python, Django, DRF, JWT, Docker

## Как запустить проект в dev-режиме:

Клонировать репозиторий и перейти в него в командной строке:
```
git clone git@github.com:Leenominai/infra_sp2.git
cd infra_sp2
```
Cоздать и активировать виртуальное окружение:
```
py -3.7 -m venv venv
venv/Scripts/activate
```
Привести расположение файлов в порядок в соответствии и ТЗ (будет необходимо перенести часть файлов проекта api_yamdb):
* файлы для сборки образа перенесите в папку приложения;
* файлы для развёртывания инфраструктуры поместите в папку infra.'

Структура проекта должна выглядеть следующим образом:
```
infra_sp2
├── api_yamdb/
│    ├── api/ <-- Директория приложения api
│    │   └── # Файлы приложения
│    ├── api_yamdb/
│    │   ├── __init__.py
│    │   ├── settings.py
│    │   ├── urls.py
│    │   └── wsgi.py
│    ├── core/ <-- Директория приложения core
│    │   └── # Файлы приложения
│    ├── reviews/ <-- Директория приложения reviews
│    │   └── # Файлы приложения
│    ├── users/ <-- Директория приложения users
│    │   └── # Файлы приложения
│    ├── static <-- Директория для сборки статических файлов проекта
│    │   └── redoc.yaml
│    ├── templates
│    │   └── redoc.html
|    ├── Dockerfile 
│    ├── manage.py
|    └── requirements.txt 
├── infra/ <-- Директория с файлами для развёртывания инфраструктуры
│    ├── nginx/ 
│    │   └── default.conf
│    ├── .env
|    └── docker-compose.yaml
├── tests/ <-- Тесты для проверки работы перед сдачей уже будут в репозитории
├── .gitignore
├── pytest.ini <-- Файл уже будет в репозитории
├── README.md
└── setup.cfg <-- Файл уже будет в репозитории 
```
Следующие манипуляции проводим, начиная проводить манипуляции в корневом каталоге ../infra_sp2/
Перейти в папку api_yamdb и установить зависимости из файла requirements.txt:
```
cd api_yamdb
pip install -r requirements.txt
```
Дописать и изменить следующие файлы конфигураций для Docker'а:
* Dockerfile: проверьте пути до локальных файлов и поправьте их, если это необходимо.
* docker-compose.yaml: в этом файле тоже проверьте пути. Особое внимание уделите инструкции build контейнера web. Докерфайл для сборки этого контейнера больше не лежит в одной директории с файлом docker-compose.yaml. Задайте новый путь до докерфайла.
* settings.py: укажите дефолтные значения для переменных из env-файла, ведь в env-файле хранится секретная информация, а ей не место в репозитории. Дефолтные значения нужны для успешного прохождения тестов на платформе.

Разворачиваем контейнеры:
```
cd infra
sudo docker-compose up
```
Выполняем миграции и создаём суперпользователя:
```
sudo docker-compose run web python3 manage.py migrate
docker-compose exec web python manage.py createsuperuser
```
Подгружаем статику к нашему контейнеру:
```
docker-compose exec web python manage.py collectstatic --no-input
```
Авторизуемся и создаём пару записей через admin-консоль:
```
http://localhost/admin/
```
Создаём дамп (резервную копию) базы:
```
docker-compose exec web python manage.py dumpdata > fixtures.json
```

## Документация:
Доступна по адресу /redoc/

## Доступные запросы к API (можно посмотреть через браузер):

+ http://localhost/api/v1/categories/ - просмотр и создание категорий
    Тип запроса: GET (доступно без токена), POST (администратор)
    Передаваемые данные (POST): {"name": string, 
                                 "slug": string}

+ http://localhost/api/v1/categories/{slug}/ - удаление категории
    Тип запроса: DELETE (администратор)

+ http://localhost/api/v1/genres/ - просмотр и создание жанров
    Тип запроса: GET (доступно без токена), POST (администратор)
    Передаваемые данные (POST): {"name": string, 
                                 "slug": string}

+ http://localhost/api/v1/genres/{slug}/ - удаление жанра
    Тип запроса: DELETE (администратор)

+ http://localhost/api/v1/titles/ - просмотр и создание произведений
    Тип запроса: GET (доступно без токена), POST (администратор)
    Передаваемые данные (POST): {"name": string,
                                 "year": integer, 
                                 "genre": array of strings,
                                 "category": string,
                                 "description": string}

+ http://localhost/api/v1/titles/{title_id} - просмотр, редактирование и удаление произведений
    Тип запроса: GET (доступно без токена), PATCH, DELETE (администратор)
    Передаваемые данные (PATCH): {"name": string,
                                 "year": integer, 
                                 "genre": array of strings,
                                 "category": string,
                                 "description": string}


## Автор текущей литерации проекта:
- [Александр Рассыхаев Python-разработчик](https://github.com/Leenominai)
