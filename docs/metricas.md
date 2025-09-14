# 📈 Configuración de Node Exporter y recogida de métricas

En esta sección configuraremos **Node Exporter** para exponer métricas del sistema y conectarlas a **Prometheus**.

---

## Instalación de Node Exporter

### Crear usuario
```bash
sudo useradd --no-create-home --shell /bin/false node_exporter
```

### Descargar Node Exporter
```bash
curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.1.2/node_exporter-1.1.2.linux-amd64.tar.gz
```

### Descomprimir y copiar binario
```bash
tar xvf node_exporter-1.1.2.linux-amd64.tar.gz

sudo cp node_exporter-1.1.2.linux-amd64/node_exporter /usr/local/bin
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter

rm -rf node_exporter-1.1.2.linux-amd64.tar.gz node_exporter-1.1.2.linux-amd64
```

### Crear servicio node_exporter
```bash
rm -rf node_exporter-1.1.2.linux-amd64.tar.gz node_exporter-1.1.2.linux-amd64
```

### Contenido del servicio:
```bash
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

### Arrancar y habilitar servicio
```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl status node_exporter
sudo systemctl enable node_exporter
```

---

## Configuración en Prometheus

### Editar configuración de Prometheus
```bash
sudo vim /etc/prometheus/prometheus.yml
```

### Añadir el nuevo job:
```bash
- job_name: 'node_exporter'
  scrape_interval: 5s
  static_configs:
    - targets: ['localhost:9100']
```

### Reiniciar servicio Prometheus
```bash
sudo systemctl restart prometheus
sudo systemctl status prometheus
```

### Verificación

Accede a http://localhost:9100/metrics para comprobar que Node Exporter está publicando métricas.

En la interfaz de Prometheus, revisa que el target node_exporter aparece como UP.