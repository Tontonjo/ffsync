# Firefox Sync Server Docker Container

## Tonton Jo  
### Join the community:
[![Youtube](https://badgen.net/badge/Youtube/Subscribe)](http://youtube.com/channel/UCnED3K6K5FDUp-x_8rwpsZw?sub_confirmation=1)
[![Discord Tonton Jo](https://badgen.net/discord/members/h6UcpwfGuJ?label=Discord%20Tonton%20Jo%20&icon=discord)](https://discord.gg/h6UcpwfGuJ)
### Support my work, give a thanks and help the youtube channel:
[![Ko-Fi](https://badgen.net/badge/Buy%20me%20a%20Coffee/Link?icon=buymeacoffee)](https://ko-fi.com/tontonjo)
[![Infomaniak](https://badgen.net/badge/Infomaniak/Affiliated%20link?icon=K)](https://www.infomaniak.com/goto/fr/home?utm_term=6151f412daf35)
[![Express VPN](https://badgen.net/badge/Express%20VPN/Affiliated%20link?icon=K)](https://www.xvuslink.com/?a_fid=TontonJo)  

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

## Control:
```shell
docker logs -f ffsync
```  
try to start a sync  if nothing move:  
- Else you're all good
- Else your Firefox cant reach your server and you should control your network / DNS settings

if you get this error in log, ensure application URL match Public URL
```
ERROR:syncserver:The public_url setting doesn't match the application url.
This will almost certainly cause authentication failures!
    public_url setting is: http://localhost:5000
    application url is:    http://fqdn.to.server:5000
```
