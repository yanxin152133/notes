# 1. webdav

## 1.1. docker 搭建 webdav
运行命令：    

```bash
docker run --name webdav \
  --restart=unless-stopped \
  -p 80:80 \
  -v /home/download:/media \
  -e USERNAME=webdav \
  -e PASSWORD=yan1+ \
  -e TZ=Europe/Madrid  \
  -e UDI=1000 \
  -e GID=1000 \
  -d  ugeek/webdav:amd64
```