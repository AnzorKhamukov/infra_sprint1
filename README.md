# Kittygram
___
## Проект для любителей фотографий котиков.
Можно добавлять фотографии котиков, и их достижения, с указанием имени, цвета и возраста любимца.
___
> Установка на ос Linux на удаленном сервере
>1. Клонировать репозиторий:
=======

>>`git clone git@github.com:AnzorKhamukov/infra_sprint1.git`

>2. Перейти в папку с проектом:

>>`cd infra_sprint1`

>3.Установить виртуальное окружение для проекта:
>>`python3 -m venv venv`

>4. Активировать виртуальное окружение для проекта:

>>`source venv/bin/activate`

>5. Установить зависимости:

>>`python3 -m pip install --upgrade pip`

>>`pip install -r requirements.txt`

>6. Выполнить миграции на уровне проекта и создать суперпользователя:

>>`cd backend`

>>`python3 manage.py migrate`

>>`python3 manage.py createsuperuser`

>7. Заполнить параметры в settings.py по примеру .env.sample:

>8. Установить node.js:

>> `curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt-get install -y nodejs`

>9. Перейти в infra_sprint1/frontend и установить зависимости для frontend-приложения:

>> `npm i`

>10. Для backend-приложения установить WSGI-сервер gunicorn:

>> `pip install gunicorn`

>11. Создать файл конфигурации для автозапуска WSGi-сервера:

>> `sudo nano /etc/systemd/system/gunicorn.service`

>12. Запустить созданную службу и внести её в автозапуск:

>>` sudo systemctl start gunicorn` `sudo systemctl enable gunicorn` 

>13. Установить веб-сервер и запустить его:

>> `sudo apt install nginx -y` `sudo systemctl start nginx `

>14. Открыть порты для фаерволла и активировать его:

>> `sudo ufw allow 'Nginx Full'` `sudo ufw allow OpenSSH` `sudo ufw enable`

>15. Перейти в infra_sprint1/frontend/, скомпилировать frontend-приложение и копировать его в системную директорию веб-сервера для корректной работы статики

>> `npm run build` `sudo cp -r <имя_пользователя>/infra_sprint1/frontend/build/. /var/www/infra_sprint1/`

>16. Перейти в infra_sprint1/backend/, собрать статику для админки Django и скопировать его в системную директорию веб-сервера и изменить название папки со статикой что бы  избежать конфликта имён:

>> `STATIC_URL = 'static_backend'` `STATIC_ROOT = BASE_DIR / 'static_backend'`

>> `python manage.py collectstatic` `sudo cp -r infra_sprint1/backend/static_backend/ /var/www/infra_sprint1/`

>17. Создать папку для медиафайлов в директории веб-сервера, изменить права доступа:

>> `cd /var/www/infra_sprint1/` `mkdir media` `sudo chown -R <имя_пользователя> /var/www/infra_sprint1/media/`

>18. Перейти в файл конфигурации веб-сервера и изменить его настройки на следующие:

>> `sudo nano /etc/nginx/sites-enabled/default `

>> `server {

    server_name server_name <публичный-IP-адрес> <доменное-имя>;

    location /api/ {
        proxy_pass http://127.0.0.1:8000;
    }

    location /admin/ {
        proxy_pass http://127.0.0.1:8000;
    }

    location /media/ {
        alias /var/www/infra_sprint1/media/;
    }

    location / {
    root    /var/www/infra_sprint1;
    index   index.html index.htm;
    try_files  $uri /index.html;
    }

}` 

>19. Проверить файл конфигурации веб-сервера и перезагрузить:

>> `sudo nginx -t` `sudo systemctl reload nginx`

>20. Чтобы получить SSL-сертификат для вашего доменного имени при помощи certbot:

>> `sudo apt install snapd` `sudo snap install core; sudo snap refresh core` `sudo snap install --classic certbot`
>> `sudo ln -s /snap/bin/certbot /usr/bin/certbot ` `sudo certbot --nginx`

___
# Технологии
+ Python 3.x
+ node.js 9.x.x
+ frontend: React
+ backend: Django
+ nginx
+ gunicorn___
