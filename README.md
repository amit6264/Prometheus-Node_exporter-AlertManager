<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Prometheus Monitoring Setup — README</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;600;700&family=Syne:wght@400;600;700;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0d1117;
    --surface: #161b22;
    --surface2: #1c2330;
    --border: #30363d;
    --accent: #e6530a;
    --accent2: #f0883e;
    --green: #3fb950;
    --blue: #58a6ff;
    --purple: #bc8cff;
    --text: #e6edf3;
    --muted: #8b949e;
    --code-bg: #0d1117;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Syne', sans-serif;
    line-height: 1.7;
    min-height: 100vh;
  }

  /* Header */
  header {
    background: linear-gradient(135deg, #1a0a02 0%, #0d1117 50%, #0a0d1a 100%);
    border-bottom: 1px solid var(--border);
    padding: 48px 40px 36px;
    position: relative;
    overflow: hidden;
  }
  header::before {
    content: '';
    position: absolute;
    top: -60px; right: -60px;
    width: 300px; height: 300px;
    background: radial-gradient(circle, rgba(230,83,10,0.15) 0%, transparent 70%);
    pointer-events: none;
  }
  .badge {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    background: rgba(230,83,10,0.15);
    border: 1px solid rgba(230,83,10,0.3);
    color: var(--accent2);
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 11px;
    font-weight: 600;
    letter-spacing: 1px;
    text-transform: uppercase;
    margin-bottom: 16px;
  }
  .badge::before { content: '●'; font-size: 8px; }
  header h1 {
    font-size: clamp(24px, 4vw, 42px);
    font-weight: 800;
    letter-spacing: -1px;
    line-height: 1.1;
    margin-bottom: 12px;
  }
  header h1 span { color: var(--accent); }
  .arch-flow {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-top: 16px;
    flex-wrap: wrap;
  }
  .arch-node {
    background: var(--surface2);
    border: 1px solid var(--border);
    padding: 5px 14px;
    border-radius: 6px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: var(--blue);
  }
  .arch-arrow { color: var(--muted); font-size: 14px; }

  /* Layout */
  .container { max-width: 960px; margin: 0 auto; padding: 40px 24px; }

  /* Step card */
  .step {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    margin-bottom: 24px;
    overflow: hidden;
    transition: border-color 0.2s;
  }
  .step:hover { border-color: rgba(230,83,10,0.4); }

  .step-header {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 18px 24px;
    border-bottom: 1px solid var(--border);
    background: var(--surface2);
  }
  .step-num {
    width: 32px; height: 32px;
    background: var(--accent);
    color: white;
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    font-weight: 700;
    flex-shrink: 0;
  }
  .step-title {
    font-size: 16px;
    font-weight: 700;
    letter-spacing: 0.2px;
  }

  .step-body { padding: 20px 24px; }

  /* Sub-steps */
  .sub { margin-bottom: 20px; }
  .sub:last-child { margin-bottom: 0; }
  .sub-label {
    font-size: 12px;
    font-weight: 600;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 1px;
    margin-bottom: 8px;
  }
  p.note {
    font-size: 14px;
    color: var(--muted);
    margin-bottom: 10px;
  }

  /* Code blocks */
  .code-wrap {
    position: relative;
    margin: 8px 0;
  }
  pre {
    background: var(--code-bg);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 14px 16px;
    overflow-x: auto;
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    line-height: 1.6;
    color: #c9d1d9;
  }
  /* Syntax-like coloring */
  pre .cmd { color: var(--green); }
  pre .flag { color: var(--purple); }
  pre .val { color: var(--accent2); }
  pre .comment { color: var(--muted); font-style: italic; }
  pre .key { color: var(--blue); }

  .copy-btn {
    position: absolute;
    top: 8px; right: 8px;
    background: var(--surface2);
    border: 1px solid var(--border);
    color: var(--muted);
    padding: 4px 10px;
    border-radius: 6px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    cursor: pointer;
    transition: all 0.15s;
    z-index: 1;
  }
  .copy-btn:hover { background: var(--accent); color: white; border-color: var(--accent); }
  .copy-btn.copied { background: var(--green); color: #0d1117; border-color: var(--green); }

  /* Info box */
  .info-box {
    background: rgba(88,166,255,0.08);
    border: 1px solid rgba(88,166,255,0.2);
    border-radius: 8px;
    padding: 12px 16px;
    font-size: 13px;
    color: var(--blue);
    margin: 10px 0;
    font-family: 'JetBrains Mono', monospace;
  }
  .info-box a { color: var(--blue); }

  .warn-box {
    background: rgba(230,83,10,0.08);
    border: 1px solid rgba(230,83,10,0.25);
    border-radius: 8px;
    padding: 12px 16px;
    font-size: 13px;
    color: var(--accent2);
    margin: 10px 0;
  }

  /* Footer */
  footer {
    text-align: center;
    padding: 32px;
    color: var(--muted);
    font-size: 13px;
    border-top: 1px solid var(--border);
  }
  footer span { color: var(--accent); }

  /* Copy-all README btn */
  .readme-btn {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: var(--accent);
    color: white;
    border: none;
    padding: 10px 22px;
    border-radius: 8px;
    font-family: 'Syne', sans-serif;
    font-size: 14px;
    font-weight: 700;
    cursor: pointer;
    margin-bottom: 32px;
    transition: background 0.15s, transform 0.1s;
  }
  .readme-btn:hover { background: var(--accent2); transform: translateY(-1px); }
  .readme-btn.copied { background: var(--green); color: #0d1117; }
</style>
</head>
<body>

<header>
  <div class="badge">DevOps Guide</div>
  <h1>Prometheus + Node Exporter +<br><span>Alertmanager + Slack Alerts</span></h1>
  <div class="arch-flow">
    <div class="arch-node">Node Exporter</div>
    <span class="arch-arrow">→</span>
    <div class="arch-node">Prometheus</div>
    <span class="arch-arrow">→</span>
    <div class="arch-node">Alertmanager</div>
    <span class="arch-arrow">→</span>
    <div class="arch-node">#slack-channel</div>
  </div>
</header>

<div class="container">

  <button class="readme-btn" onclick="copyReadme(this)">📋 Copy Full README.md</button>

  <!-- STEP 1 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">01</div>
      <div class="step-title">Install Prometheus on EC2</div>
    </div>
    <div class="step-body">

      <div class="sub">
        <div class="sub-label">1.1 — Update System</div>
        <div class="code-wrap">
          <pre>sudo apt update -y</pre>
          <button class="copy-btn" onclick="copyCode(this)">Copy</button>
        </div>
      </div>

      <div class="sub">
        <div class="sub-label">1.2 — Download Prometheus</div>
        <div class="code-wrap">
          <pre>cd /tmp
wget https://github.com/prometheus/prometheus/releases/download/v2.48.0/prometheus-2.48.0.linux-amd64.tar.gz
tar -xvf prometheus-2.48.0.linux-amd64.tar.gz
sudo mv prometheus-2.48.0.linux-amd64 /usr/local/prometheus</pre>
          <button class="copy-btn" onclick="copyCode(this)">Copy</button>
        </div>
      </div>

      <div class="sub">
        <div class="sub-label">1.3 — Create Prometheus User</div>
        <div class="code-wrap">
          <pre>sudo useradd --no-create-home --shell /bin/false prometheus</pre>
          <button class="copy-btn" onclick="copyCode(this)">Copy</button>
        </div>
      </div>

      <div class="sub">
        <div class="sub-label">1.4 — Create Directories</div>
        <div class="code-wrap">
          <pre>sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus</pre>
          <button class="copy-btn" onclick="copyCode(this)">Copy</button>
        </div>
      </div>

      <div class="sub">
        <div class="sub-label">1.5 — Copy Binaries</div>
        <div class="code-wrap">
          <pre>sudo cp /usr/local/prometheus/prometheus /usr/local/bin/
sudo cp /usr/local/prometheus/promtool /usr/local/bin/</pre>
          <button class="copy-btn" onclick="copyCode(this)">Copy</button>
        </div>
      </div>

      <div class="sub">
        <div class="sub-label">1.6 — Copy Console Files</div>
        <div class="code-wrap">
          <pre>sudo cp -r /usr/local/prometheus/consoles /etc/prometheus/
sudo cp -r /usr/local/prometheus/console_libraries /etc/prometheus/</pre>
          <button class="copy-btn" onclick="copyCode(this)">Copy</button>
        </div>
      </div>

    </div>
  </div>

  <!-- STEP 2 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">02</div>
      <div class="step-title">Configure Prometheus</div>
    </div>
    <div class="step-body">
      <p class="note">Edit the config file:</p>
      <div class="code-wrap">
        <pre>sudo nano /etc/prometheus/prometheus.yml</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Paste this configuration (replace IPs):</p>
      <div class="code-wrap">
        <pre>global:
  scrape_interval: 10s

rule_files:
  - "node_alerts.yml"

alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - "&lt;ALERTMANAGER_PRIVATE_IP&gt;:9093"

scrape_configs:
- job_name: 'prometheus'
  static_configs:
  - targets: ['localhost:9090']

- job_name: 'node_exporter_metrics'
  static_configs:
  - targets: ['&lt;NODE_EXPORTER_PRIVATE_IP&gt;:9100']</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <div class="warn-box">⚠️ Replace <code>&lt;ALERTMANAGER_PRIVATE_IP&gt;</code> and <code>&lt;NODE_EXPORTER_PRIVATE_IP&gt;</code> with your actual private IPs.</div>
    </div>
  </div>

  <!-- STEP 3 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">03</div>
      <div class="step-title">Create Alert Rules</div>
    </div>
    <div class="step-body">
      <div class="code-wrap">
        <pre>sudo nano /etc/prometheus/node_alerts.yml</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Paste this alert rule:</p>
      <div class="code-wrap">
        <pre>groups:
- name: node_alerts
  rules:
  - alert: NodeExporterDown
    expr: up{job="node_exporter_metrics"} == 0
    for: 30s
    labels:
      severity: critical
    annotations:
      summary: "Node Exporter is DOWN"
      description: "Node Exporter on {{ $labels.instance }} is not reachable."</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
    </div>
  </div>

  <!-- STEP 4 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">04</div>
      <div class="step-title">Start Prometheus as a Service</div>
    </div>
    <div class="step-body">
      <p class="note">Create the systemd service file:</p>
      <div class="code-wrap">
        <pre>sudo nano /etc/systemd/system/prometheus.service</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Paste the service definition:</p>
      <div class="code-wrap">
        <pre>[Unit]
Description=Prometheus Monitoring
After=network.target

[Service]
User=prometheus
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/
Restart=always

[Install]
WantedBy=multi-user.target</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Reload and start:</p>
      <div class="code-wrap">
        <pre>sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
sudo systemctl status prometheus</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <div class="info-box">🔗 Prometheus UI → http://&lt;PROMETHEUS_PUBLIC_IP&gt;:9090</div>
    </div>
  </div>

  <!-- STEP 5 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">05</div>
      <div class="step-title">Install Node Exporter on Another EC2</div>
    </div>
    <div class="step-body">
      <div class="code-wrap">
        <pre>cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar -xvf node_exporter-1.7.0.linux-amd64.tar.gz
sudo mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Create user:</p>
      <div class="code-wrap">
        <pre>sudo useradd --no-create-home --shell /bin/false node_exporter</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Create service file:</p>
      <div class="code-wrap">
        <pre>sudo nano /etc/systemd/system/node_exporter.service</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Paste service definition:</p>
      <div class="code-wrap">
        <pre>[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Start service:</p>
      <div class="code-wrap">
        <pre>sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
sudo systemctl status node_exporter</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <div class="info-box">🔗 Metrics → http://&lt;NODE_PRIVATE_IP&gt;:9100/metrics</div>
    </div>
  </div>

  <!-- STEP 6 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">06</div>
      <div class="step-title">Install Alertmanager</div>
    </div>
    <div class="step-body">
      <div class="code-wrap">
        <pre>cd /tmp
wget https://github.com/prometheus/alertmanager/releases/download/v0.26.0/alertmanager-0.26.0.linux-amd64.tar.gz
tar -xvf alertmanager-0.26.0.linux-amd64.tar.gz
sudo mv alertmanager-0.26.0.linux-amd64 /usr/local/alertmanager</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Copy binaries:</p>
      <div class="code-wrap">
        <pre>sudo cp /usr/local/alertmanager/alertmanager /usr/local/bin/
sudo cp /usr/local/alertmanager/amtool /usr/local/bin/</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Create config directory:</p>
      <div class="code-wrap">
        <pre>sudo mkdir /etc/alertmanager</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
    </div>
  </div>

  <!-- STEP 7 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">07</div>
      <div class="step-title">Create Alertmanager Config (Slack Integration)</div>
    </div>
    <div class="step-body">
      <div class="code-wrap">
        <pre>sudo nano /etc/alertmanager/alertmanager.yml</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Paste config (replace Slack webhook URL):</p>
      <div class="code-wrap">
        <pre>global:
  resolve_timeout: 5m

route:
  receiver: 'slack-alert'
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 1h

receivers:
  - name: 'slack-alert'
    slack_configs:
      - channel: "#alerting"
        api_url: "https://hooks.slack.com/services/XXXXXXXX"
        send_resolved: true
        text: >
          *ALERT:* {{ .CommonAnnotations.summary }}
          *DETAIL:* {{ .CommonAnnotations.description }}</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <div class="warn-box">⚠️ Replace <code>api_url</code> with your actual Slack webhook URL from Step 8.</div>
    </div>
  </div>

  <!-- STEP 8 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">08</div>
      <div class="step-title">Slack Setup — Create Incoming Webhook</div>
    </div>
    <div class="step-body">
      <div class="info-box" style="margin-bottom:14px;">
        1️⃣ Open: https://api.slack.com/messaging/webhooks<br>
        2️⃣ Click <strong>Create App</strong> → <strong>From Scratch</strong><br>
        3️⃣ App name: <code>Prometheus Alerts</code><br>
        4️⃣ Select your Workspace<br>
        5️⃣ Go to <strong>Incoming Webhooks</strong> → Turn <strong>ON</strong><br>
        6️⃣ Click <strong>Add Webhook to Workspace</strong> → Select channel <code>#alerting</code><br>
        7️⃣ Copy the generated URL and paste it in <code>alertmanager.yml</code> as <code>api_url</code>
      </div>
      <p class="note">Generated URL format:</p>
      <div class="code-wrap">
        <pre>https://hooks.slack.com/services/T000XXXXX/B000XXXXX/XXXXXXXXXXXXXXXXXXXX</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
    </div>
  </div>

  <!-- STEP 9 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">09</div>
      <div class="step-title">Start Alertmanager as a Service</div>
    </div>
    <div class="step-body">
      <div class="code-wrap">
        <pre>sudo nano /etc/systemd/system/alertmanager.service</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Paste service definition:</p>
      <div class="code-wrap">
        <pre>[Unit]
Description=Alertmanager
After=network.target

[Service]
ExecStart=/usr/local/bin/alertmanager \
  --config.file=/etc/alertmanager/alertmanager.yml
Restart=always

[Install]
WantedBy=multi-user.target</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <p class="note" style="margin-top:14px;">Start service:</p>
      <div class="code-wrap">
        <pre>sudo systemctl daemon-reload
sudo systemctl start alertmanager
sudo systemctl enable alertmanager
sudo systemctl status alertmanager</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <div class="info-box">🔗 Alertmanager UI → http://&lt;ALERTMANAGER_PUBLIC_IP&gt;:9093</div>
    </div>
  </div>

  <!-- STEP 10 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">10</div>
      <div class="step-title">Test Slack Webhook</div>
    </div>
    <div class="step-body">
      <p class="note">Run this curl command (replace with your webhook URL):</p>
      <div class="code-wrap">
        <pre>curl -X POST -H 'Content-type: application/json' \
--data '{"text":"Slack Webhook Test"}' \
https://hooks.slack.com/services/T000/B000/XXXX</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <div class="info-box">✅ A test message should appear in your #alerting Slack channel.</div>
    </div>
  </div>

  <!-- STEP 11 -->
  <div class="step">
    <div class="step-header">
      <div class="step-num">11</div>
      <div class="step-title">Trigger a Real Alert (Test End-to-End)</div>
    </div>
    <div class="step-body">
      <p class="note">Stop node exporter to trigger the alert:</p>
      <div class="code-wrap">
        <pre>sudo systemctl stop node_exporter</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
      <div class="info-box" style="margin-top:14px;">
        Within 30–60 seconds:<br><br>
        🔥 <strong>Prometheus</strong> → http://&lt;PROMETHEUS_PUBLIC_IP&gt;:9090/alerts → Status: <strong>FIRING</strong><br>
        🔔 <strong>Alertmanager</strong> → http://&lt;ALERTMANAGER_PUBLIC_IP&gt;:9093 → Alert visible<br>
        💬 <strong>Slack</strong> → Message arrives in <code>#alerting</code> channel
      </div>
      <p class="note" style="margin-top:14px;">Re-start node exporter when done testing:</p>
      <div class="code-wrap">
        <pre>sudo systemctl start node_exporter</pre>
        <button class="copy-btn" onclick="copyCode(this)">Copy</button>
      </div>
    </div>
  </div>

</div>

<footer>
  Prometheus Monitoring Guide &nbsp;·&nbsp; Node Exporter <span>v1.7.0</span> &nbsp;·&nbsp; Prometheus <span>v2.48.0</span> &nbsp;·&nbsp; Alertmanager <span>v0.26.0</span>
</footer>

<textarea id="readme-content" style="position:absolute;left:-9999px;top:-9999px;" readonly>
# Prometheus + Node Exporter + Alertmanager + Slack Alerts

> Full monitoring stack setup on EC2 with Slack alerting.

## Architecture

```
Node Exporter → Prometheus → Alertmanager → Slack Channel
```

---

## Step 1 — Install Prometheus on EC2

### 1.1 Update System
```bash
sudo apt update -y
```

### 1.2 Download Prometheus
```bash
cd /tmp
wget https://github.com/prometheus/prometheus/releases/download/v2.48.0/prometheus-2.48.0.linux-amd64.tar.gz
tar -xvf prometheus-2.48.0.linux-amd64.tar.gz
sudo mv prometheus-2.48.0.linux-amd64 /usr/local/prometheus
```

### 1.3 Create Prometheus User
```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

### 1.4 Create Directories
```bash
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

### 1.5 Copy Binaries
```bash
sudo cp /usr/local/prometheus/prometheus /usr/local/bin/
sudo cp /usr/local/prometheus/promtool /usr/local/bin/
```

### 1.6 Copy Console Files
```bash
sudo cp -r /usr/local/prometheus/consoles /etc/prometheus/
sudo cp -r /usr/local/prometheus/console_libraries /etc/prometheus/
```

---

## Step 2 — Configure Prometheus

```bash
sudo nano /etc/prometheus/prometheus.yml
```

```yaml
global:
  scrape_interval: 10s

rule_files:
  - "node_alerts.yml"

alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - "<ALERTMANAGER_PRIVATE_IP>:9093"

scrape_configs:
- job_name: 'prometheus'
  static_configs:
  - targets: ['localhost:9090']

- job_name: 'node_exporter_metrics'
  static_configs:
  - targets: ['<NODE_EXPORTER_PRIVATE_IP>:9100']
```

> ⚠️ Replace `<ALERTMANAGER_PRIVATE_IP>` and `<NODE_EXPORTER_PRIVATE_IP>` with your actual private IPs.

---

## Step 3 — Create Alert Rules

```bash
sudo nano /etc/prometheus/node_alerts.yml
```

```yaml
groups:
- name: node_alerts
  rules:
  - alert: NodeExporterDown
    expr: up{job="node_exporter_metrics"} == 0
    for: 30s
    labels:
      severity: critical
    annotations:
      summary: "Node Exporter is DOWN"
      description: "Node Exporter on {{ $labels.instance }} is not reachable."
```

---

## Step 4 — Start Prometheus as a Service

```bash
sudo nano /etc/systemd/system/prometheus.service
```

```ini
[Unit]
Description=Prometheus Monitoring
After=network.target

[Service]
User=prometheus
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
sudo systemctl status prometheus
```

> 🔗 Prometheus UI → http://\<PROMETHEUS_PUBLIC_IP\>:9090

---

## Step 5 — Install Node Exporter on Another EC2

```bash
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar -xvf node_exporter-1.7.0.linux-amd64.tar.gz
sudo mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/
```

```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
```

```bash
sudo nano /etc/systemd/system/node_exporter.service
```

```ini
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
sudo systemctl status node_exporter
```

> 🔗 Metrics → http://\<NODE_PRIVATE_IP\>:9100/metrics

---

## Step 6 — Install Alertmanager

```bash
cd /tmp
wget https://github.com/prometheus/alertmanager/releases/download/v0.26.0/alertmanager-0.26.0.linux-amd64.tar.gz
tar -xvf alertmanager-0.26.0.linux-amd64.tar.gz
sudo mv alertmanager-0.26.0.linux-amd64 /usr/local/alertmanager
```

```bash
sudo cp /usr/local/alertmanager/alertmanager /usr/local/bin/
sudo cp /usr/local/alertmanager/amtool /usr/local/bin/
sudo mkdir /etc/alertmanager
```

---

## Step 7 — Create Alertmanager Config (Slack Integration)

```bash
sudo nano /etc/alertmanager/alertmanager.yml
```

```yaml
global:
  resolve_timeout: 5m

route:
  receiver: 'slack-alert'
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 1h

receivers:
  - name: 'slack-alert'
    slack_configs:
      - channel: "#alerting"
        api_url: "https://hooks.slack.com/services/XXXXXXXX"
        send_resolved: true
        text: >
          *ALERT:* {{ .CommonAnnotations.summary }}
          *DETAIL:* {{ .CommonAnnotations.description }}
```

> ⚠️ Replace `api_url` with your actual Slack webhook URL.

---

## Step 8 — Slack Setup (Create Incoming Webhook)

1. Open: https://api.slack.com/messaging/webhooks
2. Click **Create App** → **From Scratch**
3. App name: `Prometheus Alerts`
4. Select your Workspace
5. Go to **Incoming Webhooks** → Turn **ON**
6. Click **Add Webhook to Workspace** → Select channel `#alerting`
7. Copy the generated URL and paste it in `alertmanager.yml` as `api_url`

---

## Step 9 — Start Alertmanager as a Service

```bash
sudo nano /etc/systemd/system/alertmanager.service
```

```ini
[Unit]
Description=Alertmanager
After=network.target

[Service]
ExecStart=/usr/local/bin/alertmanager \
  --config.file=/etc/alertmanager/alertmanager.yml
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl start alertmanager
sudo systemctl enable alertmanager
sudo systemctl status alertmanager
```

> 🔗 Alertmanager UI → http://\<ALERTMANAGER_PUBLIC_IP\>:9093

---

## Step 10 — Test Slack Webhook

```bash
curl -X POST -H 'Content-type: application/json' \
--data '{"text":"Slack Webhook Test"}' \
https://hooks.slack.com/services/T000/B000/XXXX
```

> ✅ A test message should appear in your #alerting Slack channel.

---

## Step 11 — Trigger a Real Alert

```bash
sudo systemctl stop node_exporter
```

Within 30–60 seconds:

- 🔥 **Prometheus** → `http://<PROMETHEUS_PUBLIC_IP>:9090/alerts` → Status: **FIRING**
- 🔔 **Alertmanager** → `http://<ALERTMANAGER_PUBLIC_IP>:9093` → Alert visible
- 💬 **Slack** → Message arrives in `#alerting` channel

```bash
# Re-start after testing
sudo systemctl start node_exporter
```

---

## Component Versions

| Component | Version |
|-----------|---------|
| Prometheus | v2.48.0 |
| Node Exporter | v1.7.0 |
| Alertmanager | v0.26.0 |
</textarea>

<script>
function copyCode(btn) {
  const pre = btn.previousElementSibling;
  const text = pre.innerText;
  navigator.clipboard.writeText(text).then(() => {
    btn.textContent = '✓ Copied';
    btn.classList.add('copied');
    setTimeout(() => {
      btn.textContent = 'Copy';
      btn.classList.remove('copied');
    }, 1800);
  });
}

function copyReadme(btn) {
  const ta = document.getElementById('readme-content');
  navigator.clipboard.writeText(ta.value.trim()).then(() => {
    btn.textContent = '✓ README Copied!';
    btn.classList.add('copied');
    setTimeout(() => {
      btn.textContent = '📋 Copy Full README.md';
      btn.classList.remove('copied');
    }, 2500);
  });
}
</script>

</body>
</html>
