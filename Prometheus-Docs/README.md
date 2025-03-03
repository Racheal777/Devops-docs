# Flask API Monitoring with Prometheus & Grafana

This project integrates Prometheus and Grafana with a Flask API to monitor HTTP requests, response times, and other key metrics.

## Features
- Track total HTTP requests & response times
- Monitor API performance using Prometheus
- Visualize data in Grafana with thresholds & alerts
- Export metrics using `prometheus_flask_exporter`

## Installation & Setup

### Install Dependencies
Ensure you have Python installed, then run:
```sh
pip install flask prometheus_flask_exporter
```

### Create a Flask App with Prometheus Exporter
```python
from flask import Flask
from prometheus_flask_exporter import PrometheusMetrics

app = Flask(__name__)
metrics = PrometheusMetrics(app)

@app.route("/")
def home():
    return "Welcome to Flask Monitoring!"

@app.route("/users/signup", methods=["POST"])
def signup():
    return "User Signed Up", 201

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```
This will expose Prometheus metrics at `http://localhost:5000/metrics`.

### Run Flask App
```sh
python app.py
```

## Setting Up Prometheus

### Install Prometheus
Set up the Docker configuration for Prometheus and Grafana.

### Configure `prometheus.yml`
Create a `prometheus.yml` file:
```yaml
# global config
global:
  scrape_interval: 15s
  scrape_timeout: 10s
  evaluation_interval: 15s

alerting:
  alertmanagers:
  - follow_redirects: true
    enable_http2: true
    scheme: http
    timeout: 10s
    api_version: v2
    static_configs:
    - targets: []

scrape_configs:
- job_name: prometheus
  honor_timestamps: true
  scrape_interval: 15s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  follow_redirects: true
  static_configs:
  - targets:
    - localhost:9090

- job_name: 'ecommerce'
  scrape_interval: 10s
  metrics_path: /metrics
  static_configs:
   - targets:
     - host.docker.internal:8090
```

### Configure Docker Compose
Create a `docker-compose.yaml` file and use Docker images for Prometheus and Grafana. Set up your password and ensure that the networks for both services are the same.

### Run Docker Compose
```sh
docker compose up -d
```
Visit `http://localhost:9090` to access the Prometheus UI.

## Setting Up Grafana

### Open Grafana
Open Grafana at `http://localhost:3000`. Use the password set in the `docker-compose` file. The default username is `admin`.

### Add Prometheus as a Data Source
- Navigate to **Configuration** → **Data Sources**
- Select **Prometheus** and enter `http://localhost:9090`

### Create Dashboards & Panels
- Add a new panel
- Choose a visualization
- Use the following query for monitoring:

#### Query Example:
```sql
sum(rate(flask_http_request_total[5m]))
```

## Common Prometheus Queries

### Total Requests
```sql
sum(flask_http_request_total)
```

### Request Rate (Per Second)
```sql
sum(rate(flask_http_request_total[5m]))
```

### 95th Percentile Response Time
```sql
histogram_quantile(0.95, rate(flask_http_request_duration_seconds_bucket[5m]))
```

### Average Response Time
```sql
sum(rate(flask_http_request_duration_seconds_sum[5m])) / sum(rate(flask_http_request_duration_seconds_count[5m]))
```

## Troubleshooting

### Issue: No Metrics Available in Prometheus
- Ensure Flask is running and `http://localhost:8090/metrics` is accessible.
- Check Prometheus targets at `http://localhost:9090/targets`.
- Restart Prometheus after updating `prometheus.yml`.

### Issue: Grafana Panels Not Updating
- Refresh the dashboard manually.
- Check if the Prometheus data source is connected.
- Adjust query time ranges.

## Next Steps
- Add more API endpoints & custom metrics.
- Set up Grafana alerts.
- Deploy to production using Docker.

## License
This project is created by Racheal Kuranchie.

