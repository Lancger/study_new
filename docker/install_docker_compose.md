##  一、Install Compose
```bash

version="1.24.0"

curl -L "https://github.com/docker/compose/releases/download/$(version)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose


chmod +x /usr/local/bin/docker-compose

docker-compose --version
```
