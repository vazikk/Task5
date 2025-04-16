# Task5
1) Балансировка /redblue<br>
Красная страница:<br>
```
server {
        listen 91;
        root /var/www/html;

        location / {
                index red.html;

        }
```
<br>
Синяя страница:<br>

```
server {
        listen 92;
        root /var/www/html;

        location / {
                index blue.html;

        }
```
Конфигурация балансировки:
```
upstream backend {
        server task.kozow.com:91;
        server task.kozow.com:92;
}
server {
        listen 90;
        server_name _;

        location /redblue {
                proxy_pass https://backend/;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
```
Итог:<br>
![Image alt](https://github.com/vazikk/Task5/blob/main/image1.png) <br>
![Image alt](https://github.com/vazikk/Task5/blob/main/image2.png) <br>
________________________________________________
________________________________________________


