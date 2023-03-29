# B6.9.2-3
В этой роли мы устанавливаем nginx и php-fpm, настраиваем конфигурационные файлы nginx и php-fpm, создаем файл index.php с содержимым `<?php phpinfo();?>` и перезапускаем сервисы nginx и php-fpm. 
Шаблоны настраивают nginx для обработки запросов на PHP-скрипты и php-fpm для прослушивания соединений на сокете `/run/php/php7.4-fpm.sock`

Для того чтобы изменить DocumentRoot на `/opt/nginx/ansible`, нужно внести изменения в файл `nginx.conf.j2`:

```
root /opt/nginx/ansible;
```

А для того чтобы использовать handler для перезапуска nginx после изменения конфигурации, нужно добавить следующий блок в конец роли:

```
- name: Reload nginx
  become: true
  service:
    name: nginx
    state: reloaded
  listen: restart nginx
```

Теперь после изменения конфигурации nginx, Ansible будет использовать handler для перезапуска сервиса.
