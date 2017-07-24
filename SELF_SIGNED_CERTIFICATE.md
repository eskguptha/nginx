#SELF SIGNED CERTIFICATE
#Prerequisites

sudo apt-get update
sudo apt-get install nginx
Step One — Create the SSL Certificate
sudo mkdir /etc/nginx/ssl
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out
/etc/nginx/ssl/nginx.crt

As we stated above, these options will create both a key file and a certificate. We will be asked a few
questions about our server in order to embed the information correctly in the certificate.
Fill out the prompts appropriately. The most important line is the one that requests the Common Name
(e.g. server FQDN or YOUR name). You need to enter the domain name that you want to be associated
with your server. You can enter the public IP address instead if you do not have a domain name.

#Sample:
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:New York
Locality Name (eg, city) []:New York City
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Bouncy Castles, Inc.
Organizational Unit Name (eg, section) []:Ministry of Water Slides
Common Name (e.g. server FQDN or YOUR name) []:pharma.vidmed.in
Email Address []:admin@ pharma.vidmed.in
Both of the files you created will be placed in
/etc/nginx/sslStep Two — Configure Nginx to Use SSL
Your server block may look something like this:

server {
listen 80 default_server;
listen [::]:80 default_server ipv6only=on;
root /usr/share/nginx/html;
index index.html index.htm;
server_name pharma.vidmed.in;
location / {
try_files $uri $uri/ =404;
}
}

The only thing we would need to do to get SSL working on this same server block, while still allowing
regular HTTP connections, is add a these lines:

server {
listen 80 default_server;
listen [::]:80 default_server ipv6only=on;
listen 443 ssl;
root /usr/share/nginx/html;
index index.html index.htm;
server_name pharma.vidmed.in;
ssl_certificate /etc/nginx/ssl/nginx.crt;
ssl_certificate_key /etc/nginx/ssl/nginx.key;
location / {try_files $uri $uri/ =404;
}
}

When you are finished, save and close the file.

sudo service nginx restart
Step Three — Test your Setup
http://server_domain_or_IP
https://server_domain_or_IP
Done.

You have configured your Nginx server to handle both HTTP and SSL requests. This will help you
communicate with clients securely and avoid outside parties from being able to read your traffic.
