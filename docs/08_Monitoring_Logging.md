# Monitoring & Logging

- [Prometheus & Grafana](#prometheus-grafana)
- [ELK Stack (Elasticsearch, Logstash, Kibana)](#elk-stack-elasticsearch-logstash-kibana)
- [Datadog](#datadog)
- [New Relic](#new-relic)

---

## Prometheus & Grafana

### Prometheus CLI Commands

```bash title="Check Prometheus version"
prometheus --version
```
```bash title="Start Prometheus"
prometheus --config.file=prometheus.yml
```
```bash title="Reload Prometheus configuration"
curl -X POST http://localhost:9090/-/reload
```
```bash title="List active targets"
curl http://localhost:9090/api/v1/targets
```
```bash title="Query Prometheus API"
curl http://localhost:9090/api/v1/query?query=up
```
```bash title="View Prometheus metrics"
curl http://localhost:9090/metrics
```
```bash title="List running alerts"
curl http://localhost:9090/api/v1/alerts
```

### Prometheus Configuration (prometheus.yml)

```yaml
scrape_configs:
  - job_name: 'node'
    static_configs:
    - targets: ['localhost:9100']
  - job_name: 'kubernetes'
    static_configs:
    - targets: ['kube-state-metrics:8080']
```

### Prometheus Alert Manager Configuration (alertmanager.yml)

```yaml
route:
  receiver: 'slack'

receivers:
  - name: 'slack'
    slack_configs:
    - channel: '#alerts'
      send_resolved: true
      api_url: 'https://hooks.slack.com/services/your_webhook_url'
```

### Useful PromQL Queries

```bash title="Check target availability"
up
```
```bash title="CPU usage"
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) \* 100)
```
```bash title="Memory usage"
(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) \* 100
```
```bash title="HTTP request count"
sum(rate(http_requests_total[5m]))
```
```bash title="Disk space usage"
node_filesystem_free_bytes / node_filesystem_size_bytes \* 100
```
```bash title="Active Kubernetes pods"
count(kube_pod_status_phase{phase="Running"})
```

### Grafana CLI Commands

```bash title="Check Grafana version"
grafana-server -v
```
```bash title="Start Grafana"
systemctl start grafana-server
```
```bash title="Stop Grafana"
systemctl stop grafana-server
```
```bash title="Restart Grafana"
systemctl restart grafana-server
```
```bash title="Enable Grafana on boot"
systemctl enable grafana-server
```

### Grafana API Commands

```bash title="List dashboards"
curl -X GET http://localhost:3000/api/search\?query\=\&type\=dash-db -H "Authorization: Bearer <API_TOKEN>"
```
```bash title="Create dashboard"
curl -X POST http://localhost:3000/api/dashboards/db -H "Content-Type: application/json" -H "Authorization: Bearer <API_TOKEN>" --data '@dashboard.json'
```
```bash title="Add Prometheus data source"
curl -X POST http://localhost:3000/api/datasources -H "Content-Type:application/json" -H "Authorization: Bearer <API_TOKEN>" --data '{ "name":"Prometheus", "type": "prometheus", "url": "http://localhost:9090", "access":"proxy" }'
```
```bash title="List all users"
curl -X GET http://localhost:3000/api/users -H "Authorization: Bearer <API_TOKEN>"
```

### Integrating Prometheus with Grafana

1. Add Prometheus Data Source in Grafana
	- Grafana → Configuration → Data Sources → Add Prometheus
	- Set URL to http://localhost:9090
	- Click Save & Test
2. Import a Prebuilt Dashboard
	- Grafana → Dashboards → Import
    - Enter Dashboard ID from Grafana Repository
    - Select Prometheus as data source → Import
3. Create a New Dashboard
	- Grafana → Dashboards → New Dashboard
	- Add PromQL queries for visualization
	- Choose Panel Type (Graph, Gauge, Table, etc.)
	- Click Save Dashboard

### Grafana Alerting Setup

#### Create Alert in Grafana

1. Open **Dashboard → Edit Panel**
2. Click **Alert → Create Alert**
3. Set **Condition** (e.g., CPU usage > 80%)
4. Define **Evaluation Interval** (e.g., Every 1 min)
5. Configure **Notification Channels** (Slack, Email, PagerDuty, etc.)
6. Click **Save Alert**

#### Configure Slack Alerts in Grafana

1. Grafana → Alerting → Notification Channels
2. Click **Add New Channel**
3. Select **Slack**, enter **Webhook URL**
4. Click **Save & Test**

---

## ELK Stack (Elasticsearch, Logstash, Kibana)

### 1. Elasticsearch Commands

#### Elasticsearch CLI Commands

```bash title="Check Elasticsearch version"
elasticsearch --version
```
```bash title="Start Elasticsearch"
systemctl start elasticsearch
```
```bash title="Stop Elasticsearch"
systemctl stop elasticsearch
```
```bash title="Restart Elasticsearch"
systemctl restart elasticsearch
```
```bash title="Enable Elasticsearch on boot"
systemctl enable elasticsearch
```
```bash title="Check Elasticsearch status"
curl -X GET "http://localhost:9200"
```
```bash title="Cluster health"
curl -X GET "http://localhost:9200/_cluster/health?pretty"
```
```bash title="List cluster nodes"
curl -X GET "http://localhost:9200/_cat/nodes?v"
```
```bash title="List all indices"
curl -X GET "http://localhost:9200/_cat/indices?v"
```
```bash title="Delete an index"
curl -X DELETE "http://localhost:9200/index_name"
```

#### Index Management

```bash title="Create an index"
curl -X PUT "http://localhost:9200/index\_name"
```
```bash title="Search index"
curl -X GET "http://localhost:9200/index\_name/\_search?pretty"
```
```bash title="Insert a document"
curl -X PUT "http://localhost:9200/index\_name/\_doc/1" -H "Content-Type: application/json" -d '{"name": "DevOps"}'
```
```bash title="Retrieve a document"
curl -X GET "http://localhost:9200/index\_name/\_doc/1?pretty"
```
```bash title="Delete a document"
curl -X DELETE "http://localhost:9200/index\_name/\_doc/1"
```

#### Elasticsearch Query Examples

```bash title="Search for 'DevOps'"
curl -X GET "http://localhost:9200/index\_name/\_search?q=name:DevOps&pretty"
```
```bash title="Query using JSON"
curl -X GET "http://localhost:9200/index\_name/\_search" -H "Content-Type: application/json" -d '{ "query": { "match": { "name": "DevOps" } } }'
```
```bash title="Get all documents"
curl -X GET "http://localhost:9200/index\_name/\_search?pretty" -H "Content-Type: application/json" -d '{ "size": 10, "query": { "match_all": {} } }'
```

### 2. Logstash Commands

#### Logstash CLI Commands

```bash title="Check Logstash version"
logstash --version
```
```bash title="Start Logstash with a config file"
logstash -f /etc/logstash/logstash.conf
```
```bash title="Start Logstash"
systemctl start logstash
```
```bash title="Stop Logstash"
systemctl stop logstash
```
```bash title="Restart Logstash"
systemctl restart logstash
```
```bash title="Enable Logstash on boot"
systemctl enable logstash
```

#### Sample Logstash Configuration (logstash.conf)

```json
input {
  file {
    path => "/var/log/syslog" 
    start_position => "beginning"
  }
}

filter {
  grok {
    match => {
      "message" => "%{SYSLOGTIMESTAMP:timestamp} %{HOSTNAME:host} %{DATA:process}: %{GREEDYDATA:log_message}"
      }
  }
}

output { 
  elasticsearch {
    hosts => ["http://localhost:9200"] 
    index => "logstash-logs"
  }
  stdout { codec => rubydebug }
}
```

### 3. Kibana Commands

#### Kibana CLI Commands

```bash title="Check Kibana version"
kibana --version
```
```bash title="Start Kibana"
systemctl start kibana
```
```bash title="Stop Kibana"
systemctl stop kibana
```
```bash title="Restart Kibana"
systemctl restart kibana
```
```bash title="Enable Kibana on boot"
systemctl enable kibana
```

#### Kibana API Commands

```bash title="Check Kibana status"
curl -X GET "http://localhost:5601/api/status" -H "kbn-xsrf: true"
```
```bash title="List all Kibana spaces"
curl -X GET "http://localhost:5601/api/spaces/space" -H "kbn-xsrf: true"
```

```bash title="Kibana Dashboard Import Example"
curl -X POST "http://localhost:5601/api/saved\_objects/\_import" -H "kbn-xsrf: true" --form file=@dashboard.ndjson
```

### 4. Integrating ELK Stack

#### 1. Configure Elasticsearch in Kibana

- Go to Kibana → Management → Stack Management → Data Views
- Click Create Data View
- Set Index Pattern as logstash-\*
- Click Save

#### 2. Configuring Logstash to Send Logs to Elasticsearch

- Open `/etc/logstash/logstash.conf`
- Ensure the output points to Elasticsearch:

```json
output { 
  elasticsearch {
    hosts => ["http://localhost:9200"] 
    index => "logstash-logs"
  }
}
```

```bash title="Restart Logstash:"
systemctl restart logstash
```

### 5. Visualizing Logs in Kibana

- Go to Kibana → Discover
- Select logstash-\* Data View
- Apply Filters & View Logs

### 6. ELK Stack Monitoring

#### Monitor Elasticsearch Cluster Health

```bash title="Get Cluster health"
curl -X GET "http://localhost:9200/_cluster/health?pretty"
```
```bash title="Get Node health"
curl -X GET "http://localhost:9200/_cat/nodes?v"
```
```bash title="Get index health"
curl -X GET "http://localhost:9200/_cat/indices?v"
```

### Monitor Logstash Logs

```bash
tail -f /var/log/logstash/logstash-plain.log
```
```bash
journalctl -u logstash -f
```

### Monitor Kibana Logs

```bash
tail -f /var/log/kibana/kibana.log
```
```bash
journalctl -u kibana -f
```

---

## Datadog

### Datadog Agent Installation (Linux)

```bash
DD_API_KEY=your_api_key bash -c "$(curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"
```

### Enable Log Monitoring

```bash
sudo nano /etc/datadog-agent/datadog.yaml
```
```yaml
logs_enabled: true
```
```bash
systemctl restart datadog-agent
```

### Datadog Agent CLI Commands

```bash title="Check Datadog agent version"
datadog-agent version
```
```bash title="Check Datadog agent status"
datadog-agent status
```
```bash title="Run a specific check"
datadog-agent check <integration>
```
```bash title="Gather logs and configuration for"
datadog-agent flare troubleshooting
```
```bash title="Start Datadog agent"
systemctl start datadog-agent
```
```bash title="Stop Datadog agent"
systemctl stop datadog-agent
```
```bash title="Restart Datadog agent"
systemctl restart datadog-agent
```
```bash title="Enable Datadog agent on boot"
systemctl enable datadog-agent
```

### Metric Queries

```bash title="CPU usage"
avg:system.cpu.user{*}
```
```bash title="Top 5 disk users"
top(avg:system.disk.used{*}, 5, 'mean')
```

### Datadog API Commands

```bash title="List all metrics"
curl -X GET "https://api.datadoghq.com/api/v1/metrics" -H "DD-API-KEY:<API_KEY>"
```

```bash title="Submit a custom metric"
curl -X POST "https://api.datadoghq.com/api/v1/series" \
-H "DD-API-KEY: <API_KEY>" \
-H "Content-Type: application/json" \
--data '{ "series": [{ "metric": "custom.metric", "points": [[1633000000, 10]],"type": "gauge", "tags": ["env:prod"] }] }'
```

### Datadog Configuration

```yaml title="/etc/datadog-agent/datadog.yaml"
api_key: "<YOUR_API_KEY>"
site: "datadoghq.com"
apm_config:
  enabled: true
```

### Datadog Log Collection Setup

```yaml title="/etc/datadog-agent/datadog.yaml"
logs_enabled: true
```
```bash
systemctl restart datadog-agent
```

### Monitor Logs in Datadog UI

1. Go to Datadog → Logs → Live Tail
2. Filter logs by service, environment, or host

### Datadog Monitoring Commands

```bash title="Check configuration validity"
datadog-agent configcheck
```
```bash title="Get the hostname recognized by Datadog"
datadog-agent hostname
```
```bash title="Check agent health"
datadog-agent health
```

### Datadog Kubernetes Agent Installation

```bash
kubectl create secret generic datadog-secret --from-literal=api-key=<YOUR_API_KEY>
```
```bash
kubectl apply -f https://raw.githubusercontent.com/DataDog/datadog-agent/main/Dockerfiles/manifests/agent.yaml
```

### Datadog Kubernetes Monitoring

```bash title="List Datadog agent pods"
kubectl get pods -n datadog
```
```bash title="Check logs of a Datadog agent pod"
kubectl logs -n datadog <pod-name>
```
```bash title="Describe Datadog agent pod"
kubectl describe pod <pod-name> -n datadog
```

### Datadog Integrations for DevOps

```bash title="Install Docker integration"
datadog-agent integration install -t docker
```
```bash title="Install Kubernetes integration"
datadog-agent integration install -t kubernetes
```
```bash title="Install AWS integration"
datadog-agent integration install -t aws
```
```bash title="Install Prometheus integration"
datadog-agent integration install -t prometheus
```
```bash title="Install Jenkins integration"
datadog-agent integration install -t jenkins
```
```bash title="Install GitLab integration"
datadog-agent integration install -t gitlab
```

### Datadog Log Collection for Docker

```bash
docker run -d --name datadog-agent \
-e DD_API_KEY=<YOUR_API_KEY> \
-e DD_LOGS_ENABLED=true \
-e DD_CONTAINER_EXCLUDE="name:datadog-agent" \
-v /var/run/docker.sock:/var/run/docker.sock:ro \
-v /proc/:/host/proc/:ro \
-v /sys/fs/cgroup/:/host/sys/fs/cgroup:ro \
datadog/agent
```

### Datadog APM (Application Performance Monitoring)

```bash title="Check trace agent status"
datadog-agent trace-agent status
```
```bash title="Show trace agent configuration"
datadog-agent trace-agent config
```
```bash title="Restart the trace agent"
datadog-agent trace-agent restart
```

### Datadog CI/CD Monitoring

```bash title="Track CI/CD pipeline duration"
curl -X POST "https://api.datadoghq.com/api/v1/series" \
-H "DD-API-KEY: <API_KEY>" \
-H "Content-Type: application/json" \
--data '{ "series": [{ "metric": "ci.pipeline.duration", "points": [[1633000000, 30]], "type": "gauge", "tags": ["pipeline:deploy"] }] }'
```

### Datadog Synthetic Monitoring (API Tests)

```bash
curl -X POST "https://api.datadoghq.com/api/v1/synthetics/tests" \
-H "DD-API-KEY: <API_KEY>" \
-H "Content-Type: application/json" \
--data '{
  "config": { "request": { "method": "GET", "url": "https://example.com" },
  "assertions": [{ "operator": "is", "type": "statusCode", "target": 200 }] },
  "locations": ["aws:us-east-1"],
  "message": "Website should be reachable",
  "name": "Website Availability Test",
  "options": { "monitor_options": { "renotify_interval": 0 } },
  "tags": ["env:prod"],
  "type": "api"
}'
```

### Datadog Dashboard & Alerts

#### Create a new dashboard

```bash
curl -X POST "https://api.datadoghq.com/api/v1/dashboard" \
-H "Content-Type: application/json" \
-H "DD-API-KEY: <API_KEY>" \
--data '{
 "title": "DevOps Dashboard", 
 "widgets": [
    {
       "definition": {
         "type": "timeseries", 
         "requests": [
            { "q": "avg:system.cpu.user{*}" }
         ]
       }
    }
 ]
}'
```

#### Create an alert

```bash
curl -X POST "https://api.datadoghq.com/api/v1/monitor" \
-H "Content-Type: application/json" \
-H "DD-API-KEY: <API_KEY>" \
--data '{
  "name": "High CPU Usage", 
  "type": "query alert",
  "query": "avg(last_5m):avg:system.cpu.user{*} > 80", 
  "message": "CPU usage is too high!",
  "tags": ["env:prod"]
}'
```

#### Datadog Incident Management

```bash
curl -X POST "https://api.datadoghq.com/api/v1/incidents" \
-H "Content-Type: application/json" \
-H "DD-API-KEY: <API_KEY>" \
--data '{
  "data": {
  "type": "incidents",
  "attributes": {
    "title": "Production Outage",
    "customer_impact_scope": "global",
    "customer_impact_duration": 30,
    "severity": "critical",
    "state": "active",
    "commander": "DevOps Team"
    }
  }
}'
```

---

## New Relic

### Install New Relic Agent

```bash title="For Linux Servers"
curl -Ls https://download.newrelic.com/install/newrelic-cli/scripts/install.sh | newrelic install
```

### Query Logs & Metrics

#### NRQL Queries (New Relic Query Language)

```sql
SELECT average(cpuPercent) FROM SystemSample SINCE 30 minutes ago
```
```sql
SELECT count(*) FROM Transaction WHERE appName = 'my-app'
```

### New Relic Agent CLI Commands

```bash title="Check New Relic agent version"
newrelic-daemon -v
```
```bash title="Start New Relic infrastructure agent"
systemctl start newrelic-infra
```
```bash title="Stop New Relic infrastructure agent"
systemctl stop newrelic-infra
```
```bash title="Restart New Relic infrastructure agent"
systemctl restart newrelic-infra
```
```bash title="Enable agent on boot"
systemctl enable newrelic-infra
```
```bash title="View New Relic agent logs"
journalctl -u newrelic-infra -f
```

### New Relic API Commands

```bash title="List all applications"
curl -X GET "https://api.newrelic.com/v2/applications.json" -H "X-Api-Key:<API_KEY>" -H "Content-Type: application/json"
```
```bash title="List monitored servers"
curl -X GET "https://api.newrelic.com/v2/servers.json" -H "X-Api-Key:<API_KEY>"
```
```bash title="Record a deployment"
curl -X POST "https://api.newrelic.com/v2/applications/<APP_ID>/deployments.json" -H "X-Api-Key:<API_KEY>" -H "Content-Type: application/json" -d '{ "deployment": { "revision": "1.0.1", "description": "New deployment", "user": "DevOps Team" } }'
```

### New Relic Configuration

```yaml title="/etc/newrelic-infra.yml"
license_key: "<YOUR_LICENSE_KEY>"
log_file: /var/log/newrelic-infra.log
custom_attributes:
  environment: production
```

### New Relic Log Monitoring Setup

#### Enable Log Forwarding

```yaml title="Edit: /etc/newrelic-infra.yml:"
logs:
  enabled: true
  include:
  - /var/log/syslog
  - /var/log/nginx/access.log
```

```bash title="Restart the agent:"
systemctl restart newrelic-infra
```

#### View Logs in New Relic UI

- Go to New Relic → Logs
- Filter logs by application, environment, or tags

#### New Relic Monitoring Commands

```bash title="Check agent status"
newrelic-infra --status
```
```bash title="Run a diagnostic test"
newrelic-infra --test
```
