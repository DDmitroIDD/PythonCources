# Домашка по WEB

Все ваши домашки по двум следующим темам (Web, REST API) собираются в один целый проект. Так что в случае выполнения
всех домашних заданий, у вас будет еще один готовый проект.

## Суть

Цель проекта - разработка удобного в использовании веб-приложения с помощью фреймворка Django. Основная цель
состоит в создании платформы для блогов, которая позволит пользователям публиковать и управлять статьями на разные темы.
Приложение предоставит возможность авторам для создания и форматирования своих статей, а также обеспечит
беспрепятственную возможность чтения для посетителей. Добавить возможность комментировать статьи для залогиненых
пользователей

### Основные функции

#### Регистрация и Аутентификация Пользователей

На сайте будет возможность регистрироваться, логиниться, и управлять своим профилем

#### Управление Статьями

Авторы смогут создавать, редактировать и удалять статьи в рамках приложения.

Так же заложена система категоризации. Статьи могут быть помечены, как статьи на определенные темы(тему).

Пользователи же смогут подписываться на определенные темы, что ты видеть только те темы, которые им интересны.

> Существующие темы добавляет администратор, а пользователи только пользуются уже готовым списком

#### Написание комментариев

Так же предлагается возможность оставлять на портале комментарии. Для тех кто уверен в своих силах, предлагаю сделать
возможность комментировать комментарии. Но это не обязательное условие.

> Удалять или изменять комментарии мы не будем

# Сами задания

Тут в каждом подразделе будет написано, что необходимо сделать после каждого занятия, в том порядке в котором мы будем
это учить.

## Создание проекта и `URLS`

Необходимо создать проект и приготовить `urls` для будущих действий:

> На этом этапе эти адреса не должны делать ничего полезного, они только должны открываться, если их запрашивать
> (с любым текcтом, это пока не важно)

- `/` - основная страница, на которой будет список всех статей.
- `/my-feed/` - страница, на которой будут только статьи по темам, на которые подписан пользователь.

- `/<article_id>/` - Cтраница, на которой будет отображаться статья по `id`.
- `/<article_id>/comment/` - Адрес, который мы будем использовать для написания комментариев к статье.
- `/<article_id>/update/` - Страница, которую мы будем использовать для изменения существующей статьи.
- `/<article_id>/delete/` - Адрес, который мы будем использовать для удаления статьи

- `/create/` - Страница, на которой мы будем создавать новые статьи.

- `/topics/` - Страница, с перечнем всех тем на сайте
- `/topics/<topic_id>/` - Страница, со всеми статьями по определенной теме
- `/topics/<topic_id>/subscribe/` - Адрес для подписки на конкретную тему
- `/topics/<topic_id>/unsubscribe/` - Адрес для отписки от конкретной темы

- `/profile/` - Страница, с данными пользователя и перечнем его подписок
- `/register/` - Страница, регистрацией нового пользователя
- `/set-password/` - Страница, с изменением пароля
- `/login/` - Страница, для входа на сайт
- `/logout/` - Адрес для выхода с сайта

- `/<int:year>/<int:month>/` - страница, на которой будут статьи созданные в конкретный месяц. В случае запроса не
  настоящей даты, должна быть ошибка.

> Например:
> - `/2022/10/` - все хорошо
> - `/2059/12/` - тоже все хорошо, но статей нет
> - `/123/10/` - ошибка
> - `/123/14/` - ошибка
> - `/2020/22/` - ошибка

## Шаблоны (templates)

> На данном этапе страницы все еще заполнены чем-то абстрактным, работать должен только переход между страницами (
> тег `<a>`, и темплейт тег `url`)

Создать базовый шаблон, в котором должна быть указана строка навигации (перейти в список топиков, свой профиль итд.).

Все остальные страницы должны наследоваться от базового.

Для всех `urls` у которых указано слово `страница`, а не `адрес`, добавить шаблон.

У всех страниц должен быть разный `Title`.

## Models

> Все созданные модели необходимо добавить в админ панель, и через нее попробовать создать различные связанные объекты

> Создать администратора всегда можно через.
>
> `python manage.py createsuperuser`

> В качестве юзера мы будем использоваться встроенного в Django.
>
> `from django.contrib.auth.models import User`

### Topic

Модель темы, например: `Спорт`, `Кино`, `Мода`.

Поля:

- Название
- Дата создания

### Article

Статья.

- Заголовок
- Содержание статьи

### Comment

- Текст комментария
- Дата создания

### Связи

- Темы должны быть связаны с юзером через M2M (Many-to-many). Что бы пользователи могли подписываться на какие-то темы.
- У статьи должны быть темы, тоже M2M, статья может быть более чем на одну тему
- У статьи должен быть автор. Связь с пользователем ForeignKey
- У комментария должен быть автор и привязка к статье (и то и то ForeignKey)

#### Задачка для тех, кому хочется посложнее

Добавить возможность комментировать-комментарии. Для этого нужно создать самоссылочную связь.

> На этом этапе у вас должны быть страницы на которых пока нет ничего полезного. И админка в которой вы можете создать
> необходимые вам данные (статьи, пользователей, темы, комментарии)

## ORM

На этом этапе вы должны на все страницы, где предполагается отображение добавить это отображение. Пока без возможности
создавать или редактировать что либо

> Везде, где отображается список статей, должен быть список из строк, обрезанных до 50 символов и в конце `...` и где-то
> рядом с каждой статьей ее темы, которые сами по себе тоже кликабельны и переводят на список всех статей по этой теме.
> Причем
> эти строки должны быть ссылками на страницу с деталями статьи, где уже отображается все, и заголовок и статья и
> комментарии.

Так как мы пока что не умеем работать с пользователем. То список его тем или данные о его профиле, можно заполнить
каким-то статическими данными.

Например всегда при открытии моей ленты открывать статьи по темам "кино" и "спорт".

- Главная страница. Список всех статей, в обратном порядке, последняя созданная, это первая в списке.
- Страница с "твоей лентой", если у пользователя через админку добавлены темы, то должен отображаться список статей по
  этим темам, если темы не выбраны или список статей по темам пуст, то должна быть кнопка для перехода на страницу и с
  темами.
- Cтраница со списком существующих тем
- Страница с детальной информацией о статье
- Страница для обновления статьи пока должна быть такой же, как и с деталями
- Должна коректно отрабатывать страница с поиском по дате создания статьи. Если статьи есть, то отобразить, если нет
  предоставить ссылку на переход на главную

## Формы и аутентификация

Теперь мы знаем как работают пользователи, и как отправлять данные.

> Везде где для действия требуется наличие пользователя, не отображать не залогиненому пользователю кнопки и формы.
> Например создать статьи или добавить комментарий, или моя лента итд.

- Реализовать логин, логаут
- Реализовать регистрацию
- Заполняем и заставляем работать страницу для создания статей
- На страницу с деталями статьи добавить добавление комментария
- На страницу с профилем и страницу со списком статей, добавить кнопки подписаться и отписаться в зависимости от того
  подписан ли текущий пользователь на эту тему. Продумать как сделать так что бы при нажатии на эти кнопки,
  пользователь возвращался на туже самую страницу.
- Реализовать "мою ленту"
- Реализовать обновление и удаление статей, причем с делать это может только автор
- Реализовать смену пароля

## CVB

Переписать все на использование Class Base View
