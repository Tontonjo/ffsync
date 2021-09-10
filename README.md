# Firefox Sync Server Docker Container

## Tonton Jo - 2021
Join me on Youtube: https://www.youtube.com/c/tontonjo

If you find this usefull, please think about [Buying me a coffee](https://www.buymeacoffee.com/tontonjo)
and to [Subscribe to my youtube channel](http://youtube.com/channel/UCnED3K6K5FDUp-x_8rwpsZw?sub_confirmation=1)

Find here [video about firefox sync server here](https://www.youtube.com/watch?v=HTimc448BFs)

## Container link:
https://github.com/crazy-max/docker-firefox-syncserver

## Usage:

Generate a random secret:
```shell
secret=$(LC_ALL=C tr -dc 'A-Za-z0-9!#%&\()*+,-./:;<=>?@[\]^_{}~' </dev/urandom | head -c 20) \
echo $secret
```  
Use this password to set FF_SYNCSERVER_SECRET in the docker run command:
```shell
docker run -d -p 5000:5000 --name ffsync \
  -e TZ="Europe/Paris" \
  -e PUID=1000 \
  -e PGID=100 \
  -e FF_SYNCSERVER_SECRET="$secret" \
  -e FF_SYNCSERVER_FORWARDED_ALLOW_IPS=* \
  -e FF_SYNCSERVER_PUBLIC_URL=http://fqdn.to.server:5000 \
  -v /path/to/ffsync/:/data \
  crazymax/firefox-syncserver:latest
  ``` 
## Firefox configuration:
enter this url in firefox: about:config

Edit identity.sync.tokenserver.uri
from: https://token.services.mozilla.com/1.0/sync/1.5
to: http://fqdn.to.server:5000/token/1.0/sync/1.5
