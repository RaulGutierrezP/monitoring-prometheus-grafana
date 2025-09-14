# Configuración de Prometheus

Este documento explica cómo está configurado Prometheus en este proyecto y cómo añadir nuevos targets para monitorizar.

---

## Fichero principal: `prometheus.yml`

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    scrape_interval: 5s
    scrape_timeout: 4s
    static_configs:
      - targets: ["localhost:9100"]
```

## Descripción de la configuración

### Global

scrape_interval: 15s
Intervalo de scraping por defecto para todos los jobs (cada 15 segundos).

### scrape_configs

Job: prometheus

Monitorea el propio servidor de Prometheus.

Target: localhost:9090.

Job: node_exporter

Monitorea métricas del Node Exporter.

scrape_interval: 5s → Reemplaza el global, scrape cada 5 segundos.

scrape_timeout: 4s → Tiempo máximo para recibir la respuesta.

Target: localhost:9100.