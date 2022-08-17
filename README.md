# yamdb_final
![example workflow](https://github.com/Povidailoff/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

---

Стек: Python, Django, PostgreSQL, pyjwt, Docker, nginx, gunicorn.

---

## Git Actions
---
Воркфлоу состоит из четырёх шагов:

* <b>Tests</b> - Тестирование проекта по PEP8 и локальные тесты.

* <b>Build_and_push</b> - Сборка образа и отправка в репозиторий на Docker Hub

* <b>Deploy</b> - Настройки для автоматического развёртывания на сервере

* <b>Send_message</b> - В случае успешной развёртки в телеграм отправляется сообщение.

---
## Git Secrets

Для работоспособности проекта в репозиторий нужно добавить секретные данные. Секреты добавляются в <b>Settings</b> > <b>Secrets</b> > <b>Actions</b>
Все нужные ключи перечислены в файле Secrets_example.env.

---

## Разворачиваем проект

Для работы на сервере должны быть установлены пакеты для работы с докером.

```
sudo apt install docker.io docker-compose
```

Создаем подпапку для конфигурации nginx

```
mkdir nginx
```

После этого в корневую папку пользователя копируем файл docker-compose.yaml и файл конфигурации nginx

```
scp docker-compose.yaml <host>@<public ip>:.
cd nginx
scp default.conf <host>@<public ip>:./nginx
```

Пушим проект в гит и ждём деплоя

После деплоя заходим на сервер, переходим в папку с manage.py, собираем статику и выполняем миграции

```
docker-compose exec web python manage.py collectstatic --no-input
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate --noinput
```
