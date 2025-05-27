# Install with nginx for SSL

If you are using nginx and want to use ssl with docker compose setup you can modify your nginx
config to do the following


```
server {
    listen 443 ssl;
    server_name pixelview.pixelvirt.com;

    ssl_certificate /etc/letsencrypt/live/pixelview.pixelvirt.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/pixelview.pixelvirt.com/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        proxy_pass http://localhost:80;

        # WebSocket headers
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        # Standard proxy headers
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

This will terminate tls and pass your traffic back to port 80 of your local install. The websocket bit
is required for wss connection if you are using chatbot.
