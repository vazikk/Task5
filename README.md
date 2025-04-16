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

2) Переход на /image1 с jpeg и /image2 с png <br>
   Создание регулярного выражения для переворота картинок с форматом jpeg <br>
    Код:<br>

   
```
        location /image1 {
                alias /var/www/image/;
                index car.jpeg;

                location ~ \.jpeg$ {
                         image_filter rotate 180;  
                         image_filter_buffer 10M;   
                    }

        }

        location /image2 {
                alias /var/www/image/;
                index porche.png;

                location ~ \.jpeg$ {
                         image_filter rotate 180;  
                         image_filter_buffer 10M;   
                    }
        }
 ```
Итог: 
![Image alt](https://github.com/vazikk/Task5/blob/main/image3.png) <br>
![Image alt](https://github.com/vazikk/Task5/blob/main/image4.png) <br>
_______________________________________________
_______________________________________________
3) Настройка логов на показ куда проксировал запрос клиента <br>

```
log_format custom '$remote_addr - $remote_user [$time_local] "$request" '
                     '$status $body_bytes_sent "$http_referer" '
                     '"$http_user_agent" "$http_x_forwarded_for" '
                      'upstream: $upstream_addr';


        access_log /var/log/nginx/access.log custom;

```

Итог:

```
93.171.161.0 - - [16/Apr/2025:13:20:14 +0000] "GET /redblue HTTP/1.1" 200 149 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/135.0.0.0 Safari/537.36" "-" upstream: 3.94.21.113:92
```





