# Описание и инструкция к установке PostgreSQL

## Введение в СУБД

![](https://blog.skillfactory.ru/wp-content/uploads/2023/02/subd1-1409326.png)

Система управления базами данных (СУБД) — это программное обеспечение, предназначенное для управления, организации и
взаимодействия с базами данных. СУБД позволяет пользователям создавать, читать, обновлять и удалять данные в базе
данных, используя различные команды и запросы.

## Основные типы СУБД

Существует несколько основных типов СУБД:

1. **Реляционные СУБД (РСУБД)**: Основываются на реляционной модели данных, где данные организованы в таблицы. Примеры:
   PostgreSQL, MySQL, SQLite, Microsoft SQL Server, Oracle.
2. **Объектно-ориентированные СУБД**: Хранят данные в виде объектов, как в объектно-ориентированном программировании.
3. **NoSQL СУБД**: Используются для работы с неструктурированными данными. Примеры: MongoDB, Cassandra.
4. **Гибридные СУБД**: Сочетают в себе свойства реляционных и NoSQL СУБД.

> На данном этапе нас интересуют только **Реляционные СУБД (РСУБД)**

## Примеры СУБД

1. **PostgreSQL**: Мощная, открытая реляционная СУБД, поддерживающая расширенные SQL-функции.
2. **MySQL**: Одна из самых популярных реляционных СУБД, известная своей скоростью и надежностью.
3. **SQLite**: Легковесная, встраиваемая СУБД, не требующая отдельного серверного процесса.
4. **Microsoft SQL Server**: Коммерческая реляционная СУБД, разработанная Microsoft.
5. **Oracle**: Одна из самых мощных и распространенных коммерческих СУБД.

## Принцип клиент-сервер

![](https://learn.coderslang.com/client-server-rdbms.png)

Большинство СУБД, таких, как PostgreSQL и MySQL, работают по принципу клиент-сервер. Сервер отвечает за управление базой
данных, обработку запросов и обеспечение безопасности. Клиенты отправляют запросы на сервер и получают результаты.

## Установка PostgreSQL

Когда вы устанавливаете себе PostgreSQL (А это будет основная СУБД с которой мы будем работать и наиболее часто
используемая СУБД на реальных проектах с python) вы устанавливаете себе сразу и сервер, и клиент на ваш личный
компьютер.

Сервер после установки, будет запускаться вместе с компьютером (это можно отключить если необходимо). И поэтому мы чаще
всего вообще не задумываемся о запуске локально сервера для базы данных. Но это важный компонент работы системы.

### Установка на Windows

1. Загрузите установочный файл с [официального сайта PostgreSQL](https://www.postgresql.org/download/windows/).
2. Запустите установочный файл и следуйте инструкциям мастера установки.
3. Выберите компоненты для установки, включая PostgreSQL Server, pgAdmin, и psql.
4. Установите пароль для суперпользователя (по умолчанию `postgres`).
5. **Запомните пароль** и путь куда вы устанавливаете свою базу данных.
6. Завершите установку.

### Установка на macOS

1. Установите Homebrew, если он еще не установлен, с помощью команды:
   ```sh
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. Установите PostgreSQL через Homebrew:
   ```sh
   brew install postgresql
   ```
3. Запустите сервер:
   ```sh
   brew services start postgresql
   ```

### Установка на Linux

Для Ubuntu:

1. Добавьте официальный репозиторий PostgreSQL:
   ```sh
   sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
   ```
2. Импортируйте ключ репозитория:
   ```sh
   wget -qO - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
   ```
3. Обновите список пакетов и установите PostgreSQL:
   ```sh
   sudo apt-get update
   sudo apt-get install postgresql
   ```
4. Проверьте статус сервера:
   ```sh
   sudo systemctl status postgresql
   ```

## psql

`psql` — это командная строка для работы с PostgreSQL. Она позволяет выполнять SQL-запросы, скрипты и управлять базой
данных.

> Она устанавливается вместе с базой данных и является клиентом для PostgreSQL

### Настройка psql

#### Windows

К сожалению при установке на Windows, автоматически работать ничего не будет.

1. Установите PostgreSQL, как описано выше.
2. Добавьте каталог `bin` PostgreSQL в переменную PATH, чтобы можно было запускать psql из командной строки:

Вам нужно найти где именно у вас на компьютере установлена база данных. В ней будет папка с номером версии которую вы
установили, например 16.

Вам нужно скопировать полный путь к этой папке. Например: `C:\Program Files\PostgreSQL\16\bin`

После этого в терминале выполнить вот эту команду указав правильный путь:

   ```sh
   setx PATH "%PATH%;C:\Program Files\PostgreSQL\xx\bin"
   ```

#### macOS и Linux

После установки PostgreSQL через Homebrew или пакетный менеджер, psql уже будет доступен в PATH.

## Запуск и вход в `psql`

Если прошлый этап выполнен правильно, то у вас появится доступ к тому, что бы запустить команду `psql`, для это нужно
выполнить в консоли:

### Windows

```sh
psql -U postgres
```

### Linux/mac

```sh
sudo -u postgres psql
```

Линукс либо мак при такой команде сначала спросит пароль от системы и только после этого пароль от базы, это нормально.

### Пароль не печатается, что делать?

Когда вы введете команду, система спросит у вас пароль. Вы начнете его вводить, и не увидите ничего. Никакого отклика.
Это так и задумано, пароль вводится, продолжайте его набирать и в конце нажмите энтер.

> На всякий случай еще раз. ПАРОЛЬ НЕ БУДЕТ ПЕЧАТАТЬСЯ!!!

### Что будет видно если все хорошо?

Если все хорошо, то вы увидите примерно такой текст в консоли:

![](https://www.w3schools.com/postgresql/screenshot_postgresql_shell6.png)

Если вы видите что-то похожее, это значит что вы успешно завершили подготовку к занятиям по базам данных.

Удачи в изучении!