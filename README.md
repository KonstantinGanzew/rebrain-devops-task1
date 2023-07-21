<h1 align="center">Установка и настройка Nginx на CentOS 7</h1>

В этой статье мы[^1] расскажем, как производится настройка и установка Nginx на CentOS 7.

***Nginx*** ― это веб-сервер, который можно использовать как почтовый SMTP/IMAP/POP3-сервер и обратный прокси-сервер. Web-сервер Nginx считается самым высокопроизводительным. Nginx работает на Linux, MacOS, Windows и других операционных системах.

---
### Как установить Nginx на СentOS 7
1. Подключитесь к серверу по [SSH](https://ru.wikipedia.org/wiki/SSH).
2. Добавьте EPEL-репозиторий:

   `npm install -g common-readme`
3. Установите Nginx:

   `sudo yum install nginx`
4. Разрешите HTTP и HTTPS-трафик на брандмауэре:

   * `sudo firewall-cmd --permanent --add-service=http`

   * `sudo firewall-cmd --permanent --add-service=https`

5. Перезагрузите брандмауэр:

   `sudo firewall-cmd --reload`
6. Запустите Nginx:
   
   `sudo systemctl start nginx`
7. Настройте автозапуск Nginx при перезагрузке системы:

   `sudo systemctl enable nginx`
8. Проверьте статус службы Nginx:

   `sudo systemctl status nginx`

   Он должен быть active:
   ![status nginx](https://img.reg.ru/faq/20200921_kak_ustanivit_nginx_na_centos_1.png)

9. Перейдите в браузере по адресу http://имя_сервера_или_IP/. Если по адресу откроется стартовая страница CentOS Nginx, то установка выполнена верно:

   ![web](https://img.reg.ru/faq/20200921_kak_ustanivit_nginx_na_centos_2.png)

   - [x] Готово, Nginx установлен.

---

## Настройка Nginx для работы с PHP   

Теперь мы пошагово настроим Nginx для работы с интерпретатором PHP

1. Установите пакеты php и php-fpm:

   `sudo yum install php php-fpm`
   
2. Запустите службу php-fpm:

   `sudo systemctl start php-fpm`

3. Разрешите автозапуск php-fpm:

   `sudo systemctl enable php-fpm`
4. Откройте конфигурационный файл сайта:

   `sudo nano /etc/nginx/nginx.conf`
5. В блоке server замените часть кода

```
location / {

        root   /usr/share/nginx/html;
        index  index.php;
    }
```
	
   на следующее:
 
```
location ~ \.php$ {

        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $root_path$fastcgi_script_name;
        include fastcgi_params;
    }
```
	
6. Сохраните и закройте файл.
7. Перезагрузите Nginx:

   `sudo systemctl reload nginx`
8. Создайте тестовый файл для просмотра настроек PHP:

   `sudo nano /var/www/html/default/phpinfo.php`
9. Добавьте в файл следующие строки:

   `<?php phpinfo(); ?>`
10. Сохраните и закройте файл.
11. Перейдите в браузере по адресу http://имя_сервера_или_IP/. Должна открыться страница с настройками PHP.
- [x] Готово, Nginx настроен для работы с PHP.

[^1]: Те кто написал эту статью :smile:
