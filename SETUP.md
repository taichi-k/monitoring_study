# Prometheus & Grafanaによるシステムモニタリング環境構築

このリポジトリでは、Prometheusでメトリクス収集し、Grafanaで可視化するシステムモニタリング環境のサンプルを構築します。

## 構成概要
- Prometheus: メトリクス収集・保存
- Grafana: ダッシュボードによる可視化
- サンプルエクスポータ: Node Exporter（サーバの基本メトリクス収集）

## セットアップ手順

### 1. Dockerによる環境構築

#### 必要なファイル
- `docker-compose.yml`: サービス定義
- `prometheus.yml`: Prometheus設定ファイル

#### 手順
1. リポジトリ直下に以下のファイルを配置します。
2. `docker-compose up -d` で起動します。

### 2. サンプル構成ファイル

#### docker-compose.yml
```yaml
version: '3.7'
services:
  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"
  node-exporter:
    image: prom/node-exporter:latest
    ports:
      - "9100:9100"
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
```

#### prometheus.yml
```yaml
global:
  scrape_interval: 15s
scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter:9100']
```

### 3. Grafanaでダッシュボード作成
1. ブラウザで `http://localhost:3000` にアクセス（初期ユーザー: admin / admin）
2. Prometheusデータソースを追加（URL: http://prometheus:9090）
3. ダッシュボードを作成し、Node Exporterのメトリクスを可視化

---

## 参考
- [Prometheus公式](https://prometheus.io/)
- [Grafana公式](https://grafana.com/)
- [Node Exporter](https://github.com/prometheus/node_exporter)

