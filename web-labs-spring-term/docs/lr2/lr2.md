# Лабораторная работа 2. Реализация серверного приложения FastAPI

> **Для выполнения лабораторной и всех практических работ рекомендуется использовать версию Python 3.10+**

## Цели

Научится реализовывать полноценное серверное приложение с помощью фреймворка FastAPI с применением 
дополнительных средств и библиотек.

## Выбор темы

Тут надо обсудить

**Критерии модели данных:**

1. 5 или больше таблиц
2. Связи many-to-many и one-to-many
3. Ассоциативная сущность должна иметь поле, характеризующее связь, помимо ссылок на связанные таблицы


## Задание

1. **Выполнить практики 2.1-2.3** Их можно реализовать на примере, приведенном в текстах практик или используя выбранную
   тему. Практики можно предоставить в любом из ниже приведенных вариантов:
    1. Каждая практика - отдельная папка в репозитории.
    2. Каждая практика - отдельная ветка в репозитории.
    3. Каждая практика - отдельный коммит в репозитории.

2. **Задание на 9 Баллов:** Реализовать на основании выбранной модели и заданий из практик серверное приложение на
   FastAPI. Оно должно включать в себя:
    1. Таблицы, реализованные с помощью ORM SQLAlchemy или SQLModel с использованием БД PostgreSQL.
    2. API, содержащее [CRUD-ы](https://ru.wikipedia.org/wiki/CRUD). Там где это необходимо, реализовать GET-запросы
      возвращающие модели с вложенными объектами (связи many-to-many и one-to-many).
    3. Настроенную систему миграций с помощью библиотеки Alembic.
    4. Аннотацию типов в передаваемых и возвращаемых значениях в API-методах.
    5. Оформленную файловую структуру проекта с разделением кода, отвечающего за разную бизнес-логику и предметную
      область, на отдельные файлы и папки.
    6. _(опционально) Комментарии к сложным частям кода_.

3. **Задание на 15 Баллов (можно реализовывать сразу):** Реализовать задание из п.2 в асинхронном варианте.
    1. Изучить [гайд по настройке асинхронной реализации FastAPI](https://testdriven.io/blog/fastapi-sqlmodel/)
    2. Реализовать асинхронное подключение к БД с использованием asyncpg.
    3. Реализовать в асинхронном формате все эндпоинты из п.2
    4. Сделать асинхронные миграции
    !!! note "Замечание"
        Если вы реализуете код для подключения к БД по инструкции из пункта 3.а, для новых версий SQLAlchemy необходимо 
        корректно импортировать `AsyncEngine` и сопутсвующие библиотеки:
        ```py
        from sqlalchemy.ext.asyncio import AsyncEngine
        from sqlmodel import SQLModel, create_engine
        from sqlmodel.ext.asyncio.session import AsyncSession

        from sqlalchemy.orm import sessionmaker
        ```

## Отчет

Отчет по лабораторной должен содержать все реализованные эндпоинты, модели и код соединения с БД. В отчете можно 
указать исключительно финальную версию кода. Отчет должен содержать ссылки на github c папками, коммитами или ветками 
выполненных практик.  
 
## Дополнительное здание
!!! warning "Внимание!"
    Выполнение дополнительного задания возможно только после защиты ЛР 2 на 15 баллов!

**Данное задание может выполнено вместо 3ей лабораторной работы**

**Задание:** Необходимо реализовать функционал пользователя в разрабатываемом приложении. Функционал включает в себя:

1. Авторизацию и регистрацию
2. Генерацию JWT-токенов
3. Аутентификацию по JWT-токену
4. Хэширование паролей
5. Дополнительные АПИ-методы для получения информации о пользователе, списка пользователей и смене пароля
6. Асинхронность вышеперчисленного функкционала

Выполнение и защита такого задния приравнивается к выполнению третьей лабораторной работы