Bolck user from Country Block referers Page Redirect load static 404 deny ip Rewrite
# nginx
Bolck user from Country
Block referers
Page Redirect
load static
404
deny ip
Rewrite

server {
    listen   80; ## listen for ipv4; this line is default and implied
    server_name spaceio.com;

    root /home/ubuntu/domain;
    index index.html index.htm;
    client_max_body_size 5m;
    deny 54.242.109.127;
    location / {
        include proxy_params;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_pass http://localhost:8000;

        if ($allowed_country = no) {
                return 404;
               }
    }

    location /static {
    }

    location = /domain/10-interesting-facts-on-life-of-an-architect/ {
     return 301 http://domain.com/1932/the-life-of-an-architect-10-reasons-to-be-an-archi/;
    }





}

server {
    listen 80;
    server_name domain.com;

    rewrite ^/(.*) http://domain.com/$1 permanent



}


