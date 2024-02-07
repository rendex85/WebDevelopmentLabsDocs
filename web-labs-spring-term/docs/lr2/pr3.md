# Практика 2.3. Миграции, ENV, GitIgnore и структура проекта

> **Для выполнения лабораторной и всех практических работ рекомендуется использовать версию Python 3.10+**

## Миграции. Alembic

При разработке проекта может возникнуть потребность добавлять новые таблицы или изменять уже существующие. Ввиду того,
что ORM SQLModel  (и, следовательно, SQLAlchemy) не предусматривает встроенный механизм миграций, необходимо
использовать готовые библиотеки позволяющие безопасно вносить изменения в таблицы и БД проекта.

В официальной документации к FastAPI [рекомендуется](https://fastapi.tiangolo.com/tutorial/sql-databases/#alembic-note)
использовать Alembic в качестве библиотеки для менеджмента изменений в базе данных. Эта библиотека разработана специально
для ORM SQLAlchemy, и совместима с SQLModel.
Подробнее можно прочитать на [официальном сайте](https://alembic.sqlalchemy.org/en/latest/).

Для интеграции Alembic в разрабатываемый проект необходимо его установить через пакетный менеджер:
```
pip install alembic  
```

Реализация механизма миграций происходит через вызов `alembic init [name]` в командной строке, где [name] — название 
папки, хранящей настройки миграций. Сгенерируем папку с миграциями и сопутствующие файлы настроек:

```
alembic init migrations 
```
В корне проекта, помимо ранее созданных файлов должна сформироваться следующая структура:
```
migratgions/
├─ versions/
├─ env.py
├─ README
├─ script.py.mako
alembic.ini
```

Сгенерировалась папка `migratgions` хранящая внутри себя папку с файлами миграций `versions`,
файл окружения БД `env.py` и шаблон генерации миграций `script.py.mako`. В корне проекта добавился файл настроек
`alembic.ini`

Для создания миграций в SQLModel необходимо добавить импорты моделей и настроить подключение к БД

1. **В файле `alembic.ini`  переменной `sqlalchemy.url` необходимо указать адрес БД, по аналогии с тем, что находится в
   файле `connection.py`**

2. **В файле `env.py` импортировать все из `models.py`  и в переменной `target_metadata` указать
   значение `target_metadata=SQLModel.metadata`**

    ??? abstract "Пример файла"
        ```py
        
        from logging.config import fileConfig

        from sqlalchemy import engine_from_config
        from sqlalchemy import pool

        from alembic import context

        from models import *

        # this is the Alembic Config object, which provides
        # access to the values within the .ini file in use.
        config = context.config

        # Interpret the config file for Python logging.
        # This line sets up loggers basically.
        fileConfig(config.config_file_name)

        # add your model's MetaData object here
        # for 'autogenerate' support
        # from myapp import mymodel
        # target_metadata = mymodel.Base.metadata
        #target_metadata = None

        target_metadata = SQLModel.metadata

        # other values from the config, defined by the needs of env.py,
        # can be acquired:
        # my_important_option = config.get_main_option("my_important_option")
        # ... etc.


        def run_migrations_offline() -> None:
            """Run migrations in 'offline' mode.

            This configures the context with just a URL
            and not an Engine, though an Engine is acceptable
            here as well.  By skipping the Engine creation
            we don't even need a DBAPI to be available.

            Calls to context.execute() here emit the given string to the
            script output.

            """
            url = config.get_main_option("sqlalchemy.url")
            context.configure(
                url=url,
                target_metadata=target_metadata,
                literal_binds=True,
                dialect_opts={"paramstyle": "named"},
            )

            with context.begin_transaction():
                context.run_migrations()


        def run_migrations_online() -> None:
            """Run migrations in 'online' mode.

            In this scenario we need to create an Engine
            and associate a connection with the context.

            """
            connectable = engine_from_config(
                config.get_section(config.config_ini_section, {}),
                prefix="sqlalchemy.",
                poolclass=pool.NullPool,
            )

            with connectable.connect() as connection:
                context.configure(
                    connection=connection, target_metadata=target_metadata
                )

                with context.begin_transaction():
                    context.run_migrations()


        if context.is_offline_mode():
            run_migrations_offline()
        else:
            run_migrations_online()

        ```

3. **В файле `script.py.mako` импортировать библиотеку `sqlmodel`** 

4. **Добавить `level` поле модели `SkillWarriorLink`** 
    ```py
    class SkillWarriorLink(SQLModel, table=True):
        skill_id: Optional[int] = Field(
        default=None, foreign_key="skill.id", primary_key=True
        )
        warrior_id: Optional[int] = Field(
        default=None, foreign_key="warrior.id", primary_key=True
        )
        level: int | None
    ```
5. **Создать файл миграций с помощью команды `alembic revision --autogenerate -m "skill added"`**
6. **Применить миграции с помощью команды `alembic upgrade head`**

Увидеть полученные изменения можно просмотрев поля таблицы через pgAdmin.

!!! note "Замечание"
    Использование библиотеки alembic и миграций предполагает их описание, как описываются коммиты в гите. 
    При создании файлов миграции рекомендуется писать полезную информацию об изменениях в соответствующей команде
    после маркера `-m`.
## Переменные окружения и .gitignore

Настройка проекта и .env

## Приведение проекта в порядок

папки переменны и т.д.

## Практическое задание

Реализовать все на своем примере, придумать структуру проекта
