# 📊 Enhanced Monitoring API

🔍 **Advanced system monitoring and metrics collection framework** - Professional Python APIs for real-time system monitoring, memory analysis, and performance metrics with persistent data collection and alerting capabilities.

## ✨ Features

### **📈 Real-Time Monitoring**
- **Memory usage tracking** with detailed breakdown by process
- **System resource monitoring** (CPU, disk, network)
- **Performance metrics collection** with historical data
- **Real-time alerting** for threshold breaches

### **🔧 Advanced Memory Analysis**
- **Memory leak detection** and trend analysis
- **Process memory profiling** with detailed statistics
- **Cache and buffer monitoring** for optimization insights
- **Memory pressure prediction** and early warning system

### **⚡ High-Performance APIs**
- **RESTful API endpoints** for external integration
- **WebSocket support** for real-time data streaming
- **JSON/CSV export** for data analysis and reporting
- **Configurable sampling rates** for performance optimization

### **🛠️ System Integration**
- **Persistent monitoring daemon** with automatic startup
- **SystemD service integration** for production deployment
- **Log rotation and management** for long-term monitoring
- **Plugin architecture** for custom metrics collection

## 📁 Repository Structure

```
enhanced-monitoring-api/
├── core/
│   ├── enhanced-memory-api.py          # Advanced memory monitoring API
│   ├── memory-metrics-api.py           # Core metrics collection API
│   └── start-persistent-monitoring.sh  # Monitoring daemon launcher
├── endpoints/
│   ├── /api/memory/current             # Current memory status
│   ├── /api/memory/history             # Historical memory data
│   ├── /api/system/metrics             # System-wide metrics
│   └── /api/alerts/active              # Active alert status
├── config/
│   ├── monitoring.conf                 # Configuration settings
│   ├── thresholds.json                 # Alert thresholds
│   └── sampling.conf                   # Data collection intervals
└── docs/                               # API documentation and examples
```

## 🚀 Quick Start

### **Prerequisites**
```bash
# Install Python dependencies
pip install psutil flask websockets pandas numpy

# Install system monitoring tools
sudo dnf install htop iotop nethogs  # Fedora
sudo apt install htop iotop nethogs  # Ubuntu/Debian

# Install optional dependencies for advanced features
pip install prometheus_client grafana-api influxdb-client
```

### **Basic Installation**
```bash
# Clone the repository
git clone https://github.com/swipswaps/enhanced-monitoring-api.git
cd enhanced-monitoring-api

# Make scripts executable
chmod +x *.sh

# Start the monitoring API
python3 enhanced-memory-api.py

# Start persistent monitoring daemon
./start-persistent-monitoring.sh
```

### **Quick API Test**
```bash
# Test memory metrics endpoint
curl http://localhost:8080/api/memory/current

# Test system metrics endpoint
curl http://localhost:8080/api/system/metrics

# Get historical data
curl http://localhost:8080/api/memory/history?hours=24
```

## 📈 Usage Examples

### **Memory Monitoring API**
```python
import requests

# Get current memory status
response = requests.get('http://localhost:8080/api/memory/current')
memory_data = response.json()

print(f"Memory Usage: {memory_data['usage_percent']}%")
print(f"Available: {memory_data['available_gb']} GB")
print(f"Top Process: {memory_data['top_process']}")
```

### **Real-Time Monitoring**
```python
import websocket
import json

def on_message(ws, message):
    data = json.loads(message)
    print(f"Memory: {data['memory_percent']}% | CPU: {data['cpu_percent']}%")

ws = websocket.WebSocketApp("ws://localhost:8080/ws/realtime")
ws.on_message = on_message
ws.run_forever()
```

### **Historical Data Analysis**
```python
import pandas as pd
import requests

# Get 24 hours of memory data
response = requests.get('http://localhost:8080/api/memory/history?hours=24')
data = pd.DataFrame(response.json())

# Analyze memory trends
memory_trend = data['memory_percent'].rolling(window=60).mean()
print(f"Average memory usage: {memory_trend.mean():.2f}%")
```

### **Custom Alerts**
```python
import requests

# Set memory usage alert threshold
alert_config = {
    "metric": "memory_percent",
    "threshold": 85,
    "action": "email",
    "recipient": "admin@example.com"
}

response = requests.post('http://localhost:8080/api/alerts/create', json=alert_config)
```

## 🎯 API Endpoints

### **Memory Monitoring**
- `GET /api/memory/current` - Current memory status
- `GET /api/memory/history` - Historical memory data
- `GET /api/memory/processes` - Per-process memory usage
- `GET /api/memory/leaks` - Memory leak detection results

### **System Metrics**
- `GET /api/system/metrics` - Overall system metrics
- `GET /api/system/cpu` - CPU usage and load averages
- `GET /api/system/disk` - Disk usage and I/O statistics
- `GET /api/system/network` - Network interface statistics

### **Alerts & Notifications**
- `GET /api/alerts/active` - Currently active alerts
- `POST /api/alerts/create` - Create new alert rule
- `DELETE /api/alerts/{id}` - Remove alert rule
- `GET /api/alerts/history` - Alert history and logs

### **Configuration**
- `GET /api/config/current` - Current configuration
- `POST /api/config/update` - Update configuration
- `GET /api/config/thresholds` - Alert thresholds
- `POST /api/config/sampling` - Update sampling rates

## ⚙️ Configuration

### **Basic Configuration**
```json
{
  "monitoring": {
    "interval_seconds": 5,
    "history_retention_days": 30,
    "enable_alerts": true,
    "api_port": 8080
  },
  "memory": {
    "alert_threshold_percent": 85,
    "leak_detection": true,
    "process_monitoring": true
  },
  "storage": {
    "database_path": "/var/lib/monitoring/metrics.db",
    "log_rotation_days": 7,
    "compression_enabled": true
  }
}
```

### **Alert Thresholds**
```json
{
  "memory_percent": 85,
  "cpu_percent": 90,
  "disk_usage_percent": 95,
  "load_average_1min": 4.0,
  "swap_usage_percent": 50
}
```

### **Sampling Configuration**
```json
{
  "high_frequency": {
    "interval_seconds": 1,
    "metrics": ["memory", "cpu"]
  },
  "medium_frequency": {
    "interval_seconds": 5,
    "metrics": ["disk", "network"]
  },
  "low_frequency": {
    "interval_seconds": 60,
    "metrics": ["system_info", "processes"]
  }
}
```

## 🧪 Testing & Validation

```bash
# Test API endpoints
python3 -m pytest tests/test_api_endpoints.py

# Load testing
python3 tests/load_test.py --concurrent=10 --duration=60

# Memory leak testing
python3 tests/memory_leak_test.py --duration=3600

# Integration testing
python3 tests/integration_test.py
```

## 🔧 Advanced Usage

### **Custom Metrics Collection**
```python
from enhanced_memory_api import MetricsCollector

collector = MetricsCollector()

# Add custom metric
@collector.metric("custom_app_memory")
def collect_app_memory():
    # Custom collection logic
    return get_application_memory_usage()

# Register collector
collector.start()
```

### **Integration with Monitoring Systems**
```python
# Prometheus integration
from prometheus_client import start_http_server, Gauge

memory_gauge = Gauge('system_memory_percent', 'System memory usage percentage')

def update_prometheus_metrics():
    memory_data = get_memory_metrics()
    memory_gauge.set(memory_data['usage_percent'])

# Grafana dashboard integration
def export_grafana_dashboard():
    dashboard_config = generate_dashboard_config()
    upload_to_grafana(dashboard_config)
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/monitoring-enhancement`)
3. Commit your changes (`git commit -m 'Add monitoring enhancement'`)
4. Push to the branch (`git push origin feature/monitoring-enhancement`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Related Projects

- [performance-monitoring-suite](https://github.com/swipswaps/performance-monitoring-suite) - Performance analysis tools
- [memory-pressure-tools](https://github.com/swipswaps/memory-pressure-tools) - Memory management utilities
- [system-management-tools](https://github.com/swipswaps/system-management-tools) - System administration suite

## 📞 Support

For issues, questions, or contributions, please open an issue on GitHub.

---

**Built with ❤️ for comprehensive system monitoring and performance optimization**
