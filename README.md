# basic-monitoring-system

Hệ thống monitoring ở mức cơ bản. Theo dõi tài nguyên các vps linux

#### Grafana: 
  - image: grafana/grafana:12.3.0-18329792253
  - Update: 08/10/2025
  - Dashboard for node-exporter: 1860

#### Prometheus:
  - image: prom/prometheus:v3.6.0
  - Update: 22/09/2025

#### Node-exporter:
  - image: prom/node-exporter:v1.9.1
  - Update: 04/2025

#### Uptime Kuma:
  - image: louislam/uptime-kuma:nightly2
  - Update: 30/09/2025

Start grafana system

```
docker compose -f grafana-system-base.yml up -d
```


## Monitor status vps and notify with Uptime Kuma

Start kuma

```
docker compose -f kuma.yml up -d
```