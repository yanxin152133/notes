# 1. jellyfin
## 1.1. 查看显卡
```html
root@jellyfin:/# ls /dev/dri/
by-path  card0  card1  renderD128  renderD129
```

## 1.2. 运行
```html
docker run -d \
    --name=jellyfin \
    -e PUID=1000 \
    -e PGID=1000 \
    --net=host \
    -v /home/ubuntu/jellyfin/config:/config \
    -v /home/download:/data \
    -v /home/ubuntu/jellyfin/fonts:/usr/share/fonts \
    --device=/dev/dri:/dev/dri \
    --restart unless-stopped \
    lscr.io/linuxserver/jellyfin:latest
```

## 1.3. 开启硬解
jellyfin控制台开启硬件加速`Intel Quick Sync`。
