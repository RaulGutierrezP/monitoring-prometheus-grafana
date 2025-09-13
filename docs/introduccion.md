# Introducción: Instalación de Prometheus y Grafana (WSL)

## Crear los directorios para Prometheus
```bash
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```

## Descargar Prometheus (usa la versión que necesites)
```bash
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.28.0/prometheus-2.28.0.linux-amd64.tar.gz
tar xvf prometheus-2.28.0.linux-amd64.tar.gz
```

## Copiar binarios y consolas
```bash
sudo cp prometheus-2.28.0.linux-amd64/prometheus /usr/local/bin/
sudo cp prometheus-2.28.0.linux-amd64/promtool /usr/local/bin/
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
sudo cp -r prometheus-2.28.0.linux-amd64/consoles /etc/prometheus
sudo cp -r prometheus-2.28.0.linux-amd64/console_libraries /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
rm -rf prometheus-2.28.0.linux-amd64.tar.gz prometheus-2.28.0.linux-amd64
```

## Permisos y servicio systemd
```bash
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```

Crear el servicio en `/etc/systemd/system/prometheus.service` con el siguiente contenido:

```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

## Iniciar y habilitar Prometheus
```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl status prometheus
sudo systemctl enable prometheus
```

---

# Instalación de Grafana

## Instalar dependencias
```bash
sudo apt install -y apt-transport-https
sudo apt install -y software-properties-common wget
```

## Añadir repositorio de Grafana
```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
sudo apt update
```

## Instalar Grafana
```bash
sudo apt install grafana
```

## Iniciar y habilitar servicio Grafana
```bash
sudo systemctl daemon-reload
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
sudo systemctl status grafana-server
```
