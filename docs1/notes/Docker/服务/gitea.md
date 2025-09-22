# 1. gitea
## 1.1. gitea
```bash
docker run --name=gitea -d --restart=always -p 10022:22 -p 10880:3000 -v /home/ubuntu/gitea:/data -v /home/ubuntu/gitea/config:/data/gitea/conf/ gitea/gitea:latest
```

## 1.2. gitea actions
```bash
docker run \
    --restart=always \
    -v /home/ubuntu/gitea/data:/data \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e GITEA_INSTANCE_URL=<instance_url> \
    -e GITEA_RUNNER_REGISTRATION_TOKEN=<registration_token> \
    --name gitea-actions \
    -d gitea/act_runner:latest
```