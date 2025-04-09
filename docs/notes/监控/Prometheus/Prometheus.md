# 1. Prometheus

## 1.1. 组件
Prometheus 生态系统包含多个组件，其中许多是可选的：

- 主服务Prometheus Server负责抓取和存储时间序列数据
- 客户库负责检测应用程序代码
- 支持短生命周期的PUSH网关
- 基于Rails/SQL仪表盘构建器的GUI
- 多种导出工具，可以支持Prometheus存储数据转化为HAProxy、StatsD、Graphite等工具所需要的数据存储格式
- 警告管理器
- 命令行查询工具
- 其他各种支撑工具

Prometheus 大多数组件使用 Go 语言编写，易于构建和部署为二进制可执行文件。

## 1.2. 架构
![Prometheus 架构](https://prometheus.io/assets/architecture.png)

## 1.3. 安装
### 1.3.1. docker 安装 Prometheus
```bash
docker run -d --name prometheus --restart=always -p 9090:9090 -v /home/centos/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
```

`prometheus.yml`:
```yml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]
```

### 1.3.2. 可视化 Grafana
```bash
docker run -d --name=grafana --restart=always -p 3000:3000 grafana/grafana-enterprise
```

创建一个Prometheus数据源
为了创建一个Prometheus数据源Data source：

1. 点击Grafana的logo，打开工具栏。
2. 在工具栏中，点击"Data Source"菜单。
3. 点击"Add New"。
4. 数据源Type选择“Prometheus”。
5. 设置Prometheus服务访问地址（例如：http://localhost:9090）。
6. 调整其他想要的设置（例如：关闭代理访问）。
7. 点击“Add”按钮，保存这个新数据源。

### 1.3.3. 时间不同步问题
![](https://img2020.cnblogs.com/blog/2528116/202112/2528116-20211224105106903-395194182.png)

解决办法：

```bash
sudo yum install -y ntp
sudo ntpdate -u time1.aliyun.com
```

### 1.3.4. Node Exporter 
参考：[Node Exporter ](https://github.com/prometheus/node_exporter#using-docker)

#### 1.3.4.1. docker 安装 Node Exporter 
```bash
docker run -d \
  --net="host" \
  --pid="host" \
  --restart unless-stopped \
  --name=node-exporter \
  -v "/:/host:ro,rslave" \
  quay.io/prometheus/node-exporter:latest \
  --path.rootfs=/host
```

#### 1.3.4.2. prometheus.yml
修改`prometheus.yml` 文件：

```yml
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]

  - job_name: 'jellyfin'
    static_configs:
      - targets: ['192.168.3.12:9100']
        labels:
          instance: jellyfin
```

