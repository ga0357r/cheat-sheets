# Nginx
Open source web and application server.

Project Homepage: [Nginx Homepage](https://www.nginx.com/)
Documentation: [Nginx Unit Docs](https://unit.nginx.org/)

---
## Basic configuration arguments and examples

Logging and debugging:

```nginx
error_log <file> <loglevel>
    error_log logs/error.log;
    error_log logs/debug.log debug;
    error_log logs/error.log notice;
```

basic listening ports:

```nginx
listen <port> <options>
        listen 80;
        listen 443 ssl http2;
        listen 443 http3 reuseport; (this is experimental!)
```

header modifcations:
```nginx
add_header <header> <values>
        add_header Alt-svc '$http3=":<port>"; ma=<value>'; (this is experimental!)

ssl_certificate / ssl_certificate_key
        ssl_certificate cert.pem;
        ssl_certificate_key cert.key;

server_name <domains>
    server_name domain1.com *.domain1.com

root <folder>
    root /var/www/html/domain1;

index <file>
    index index.php;

location <url> {
}
    location / {
        root index.html;
        index index.html index.htm;
    }
    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }
    location ~ \\.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        include fastcgi_params;
    }
    location ~ /\\.ht {
        deny all;
    }
    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }
    location = /robots.txt {
        log_not_found off;
        access_log off;
        allow all;
    }
    location ~* .(css|gif|ico|jpeg|jpg|js|png)$ {
        expires max;
        log_not_found off;
}
```
## Reverse Proxy
### Show Client's real IP
```nginx
server {
	server_name example.com;
	location / { 
		proxy_pass http://localhost:4000;
		
		# Show clients real IP behind a proxy
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	}
}
```

## Secure your nginx docker server
### 
```nginx
server {
    # Configuration for port 443. For https
    listen 443 ssl;
    server_name localhost;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root /code;

    ssl_certificate /etc/nginx/certs/localhost.csr;
    ssl_certificate_key /etc/nginx/certs/localhost.key;
}
}
```

## return status codes
```nginx
server {
    # Configuration for port 80. Unity POB Client
    listen 80;
    server_name localhost;

    location / {
        # Redirect all HTTP requests to HTTPS(443)
	# return 301 https://$host:443$request_uri; # Permanent Redirect
        return 307 https://$host:443$request_uri; # Temporary redirect
    }
}
```

## To upgrade http to https
```nginx
server {
    # Configuration for port 80. Unity POB Client
    listen 80;
    server_name localhost;

    location / {
        # Redirect all HTTP requests to HTTPS(443)
        return 307 https://$host:443$request_uri;
    }
}

server {
    # Configuration for port 443. For https. For Unity POB Client
    listen 443 ssl;
    listen [::]:443 ssl;
    index index.php index.html;
    server_name localhost;
    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;
    root /code;

    include /etc/nginx/certs/self-signed.conf;
    include /etc/nginx/certs/ssl-params.conf;


    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass php:52379;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```
