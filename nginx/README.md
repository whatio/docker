# Docker nginx

## Command

```bash
sudo docker compose -f compose.yaml up -d --build
sudo docker compose -f compose.yaml down
sudo docker exec -it docker-nginx /bin/bash
# tail -f /usr/share/nginx/game.access.log
sudo docker logs docker-nginx -f
```

## Add `www.example.com`

### SSL

- Add [ssl/www.example.com](./ssl/www.example.com/)

### Static

- Add [static/www.example.com](./static/www.example.com/)

### Nginx config.

- Add [conf.d/www.example.com.conf](./conf.d/www.example.com.conf)
- Check all `www.example.com`

```conf
# HTTP redirect
server {
	listen      80;
	# listen      [::]:80;
	server_name www.example.com;
	return 301 https://$server_name$request_uri;
}

server {
	listen                  443 ssl;
	server_name www.example.com;
	
	root /usr/share/nginx/static/www.example.com;
	index index.html;

	# SSL
	ssl_certificate         /etc/nginx/ssl/www.example.com/www.example.com.pem;
	ssl_certificate_key     /etc/nginx/ssl/www.example.com/www.example.com.key;


	# gzip
	gzip                    on;
	gzip_vary               on;
	gzip_proxied            any;
	gzip_comp_level         6;
	gzip_types              text/plain text/css text/xml application/json application/javascript application/rss+xml application/atom+xml image/svg+xml;

	location / {
		try_files $uri $uri/ /404.html;
	}
	
}
```
