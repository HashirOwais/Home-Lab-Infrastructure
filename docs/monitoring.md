# Monitoring and Observability

A robust observability stack is crucial for maintaining the health, performance, and security of the homelab. This infrastructure utilizes a combination of best-in-class open-source tools to achieve comprehensive monitoring, covering everything from application performance metrics to security threat detection.

The primary monitoring services run on a dedicated `monitoring` VM, with security-specific monitoring handled by a separate `wazuh` VM.

## The Grafana "LGTM" Stack

The core of the observability platform is built around the Grafana stack, often referred to by the acronym **LGTM**: **L**oki for logs, **G**rafana for visualization, **T**empo for traces, and **M**imir for metrics.

-   **Grafana:** The central hub for all visualization. Grafana provides a powerful and flexible platform to create dashboards that display metrics, logs, and traces from all services in the homelab. It allows for the correlation of different data sources in a single pane of glass.

-   **Loki:** A horizontally scalable, highly available, multi-tenant log aggregation system inspired by Prometheus. Loki is designed to be very cost-effective and easy to operate. It indexes metadata about logs rather than the full text, which makes it incredibly efficient for storing and querying log data from applications and systems.

-   **Tempo:** A high-volume, minimal-dependency distributed tracing backend. Tempo is used to ingest and find traces from services instrumented with **OpenTelemetry**. It's designed to be simple to operate and deeply integrated with Grafana and Loki, allowing you to seamlessly pivot from a metric or log to the exact trace that caused it.

-   **Mimir:** A scalable long-term storage solution for Prometheus metrics. Mimir allows for the aggregation of metrics from multiple Prometheus instances (or other metric sources) into a single, globally-viewable, and highly-available datastore.

## Data Collection with OpenTelemetry (OTEL)

**OpenTelemetry** is the framework used for collecting and exporting telemetry data (metrics, logs, and traces) from applications and services. By standardizing the instrumentation, OTEL makes it possible to use a single set of APIs and libraries to send data to any compatible backend, such as the Grafana stack.

## Detailed Monitoring Setup

### Grafana Setup

Grafana is the primary tool for visualizing all collected metrics, logs, and traces. It is deployed as a Docker container.

#### Docker Compose for Grafana

```yaml
version: "3.8"
services:
  grafana:
    image: grafana/grafana
    container_name: grafana
    restart: unless-stopped
    ports:
     - '3000:3000'
    volumes:
      - grafana-storage:/var/lib/grafana
volumes:
  grafana-storage: {}
```

**Explanation:**
-   `image: grafana/grafana`: Uses the official Grafana Docker image.
-   `container_name: grafana`: Assigns a readable name to the container.
-   `restart: unless-stopped`: Ensures Grafana restarts automatically unless explicitly stopped.
-   `ports: - '3000:3000'`: Maps container port 3000 to host port 3000, making Grafana accessible via `http://<homelab_ip>:3000`.
-   `volumes: - grafana-storage:/var/lib/grafana`: Mounts a Docker volume for persistent storage of Grafana's data, dashboards, and configurations.

### Prometheus Setup

Prometheus is a powerful monitoring system that collects metrics from configured targets at given intervals, evaluates rule expressions, displays the results, and can trigger alerts if some condition is observed to be true.

#### Docker Compose for Prometheus

```yaml
version: '3.8'
services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
    ports:
      - 9090:9090
    restart: unless-stopped
    volumes:
      - ./prometheus:/etc/prometheus
      - prom_data:/prometheus
volumes:
  prom_data: {}
```

**Explanation:**
-   `image: prom/prometheus`: Uses the official Prometheus Docker image.
-   `command: - '--config.file=/etc/prometheus/prometheus.yml'`: Specifies the path to the Prometheus configuration file within the container.
-   `ports: - 9090:9090'`: Maps container port 9090 to host port 9090, making the Prometheus UI accessible.
-   `volumes:`
    -   `./prometheus:/etc/prometheus`: Maps a local directory (`./prometheus` on the host) to the container's `/etc/prometheus`, allowing you to provide your custom `prometheus.yml` configuration.
    -   `prom_data:/prometheus`: Mounts a Docker volume for persistent storage of Prometheus's time-series data.

#### Example `prometheus.yml` Configuration

You would place a `prometheus.yml` file in the `./prometheus` directory mapped above:

```yaml
global:
  scrape_interval: 15s # How frequently to scrape targets
  scrape_timeout: 10s
  evaluation_interval: 15s # How frequently to evaluate rules

alerting:
  alertmanagers:
    - static_configs:
      - targets: [] # Configure Alertmanager endpoints here
      scheme: http
      timeout: 10s
      api_version: v1

scrape_configs:
  - job_name: prometheus # Scrapes Prometheus itself
    honor_timestamps: true
    scrape_interval: 15s
    scrape_timeout: 10s
    metrics_path: /metrics
    scheme: http
    static_configs:
      - targets:
        - localhost:9090 # Prometheus's own metrics endpoint

  # Add more job configurations here for other services (e.g., Node Exporter)
```

### Node Exporter

**Node Exporter** is an agent that runs on each Linux server (VM or LXC) to expose a wide range of hardware and OS metrics. Prometheus then scrapes these metrics for storage and analysis.

-   **Installation:** Node Exporter is installed on each system that needs monitoring. This can be automated using Ansible.
-   **Metrics Collected:** CPU usage, memory usage, disk I/O, network traffic, system load, and more.

Once Prometheus is collecting metrics from Node Exporter, you can import pre-built dashboards in Grafana (e.g., Node Exporter Full Dashboard ID 1860 from grafana.com) to visualize the data.

## Security Monitoring with Wazuh

In addition to performance and application monitoring, a dedicated **Wazuh** VM provides robust security monitoring capabilities. Wazuh is an open-source security platform that combines the functions of a **Security Information and Event Management (SIEM)** and an **Extended Detection and Response (XDR)** solution.

**Key Features:**

-   **Log Data Analysis:** Collects, aggregates, and analyzes log data from across the infrastructure to identify security events.
-   **Intrusion and Malware Detection:** Monitors systems for signs of intrusion, rootkits, and malware.
-   **File Integrity Monitoring:** Alerts on changes to critical system files.
-   **Vulnerability Detection:** Scans for known vulnerabilities in the software running on monitored systems.
-   **Active Response:** Can be configured to automatically respond to threats, such as by blocking malicious IP addresses.

By combining the Grafana stack for observability with Wazuh for security, this homelab has a comprehensive, multi-layered monitoring strategy that provides deep insights into both operational health and security posture.
