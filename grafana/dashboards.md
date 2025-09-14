# Dashboards de Grafana

Este directorio contiene dashboards de Grafana exportados en formato JSON. Los dashboards permiten visualizar métricas de Prometheus de manera clara y configurable.

---

## Dashboard Exportado

Archivo: `Node Exporter Personalizado 1`  
UID: `d25878ac-007e-441f-bc50-e62440ff9eec`  
Versión: 13

Este dashboard contiene los siguientes paneles para monitorear:

1. **CPU Usage** (Gauge)  
   **Query A:**  
   ```promql
   (((count(count(node_cpu_seconds_total{instance="$node"}) by (cpu))) - avg(sum by (mode)(rate(node_cpu_seconds_total{mode='idle',instance="$node"}[$__rate_interval])))) * 100) / count(count(node_cpu_seconds_total{instance="$node"}) by (cpu))
   ```

2. **RAM Usage**
   **Query A:**  
   ```promql
   100 - ((node_memory_MemAvailable_bytes{instance="$node"} * 100) / node_memory_MemTotal_bytes{instance="$node"})
   ```

3. **CPU Gráfico** (Time Series)
   **Query A: Busy system**  
   ```promql
   sum by (instance)(rate(node_cpu_seconds_total{mode="system",instance="$node"}[$__rate_interval])) * 100
   ```

   **Query B: Busy User**  
   ```promql
   sum by (instance)(rate(node_cpu_seconds_total{mode='user',instance="$node"}[$__rate_interval])) * 100
   ```

   **Query C: Busy Iowait** 
   ```promql
   sum by (instance)(rate(node_cpu_seconds_total{mode='iowait',instance="$node"}[$__rate_interval])) * 100
   ``` 

   **Query D: Busy IRQs**  
   ```promql
   sum by (instance)(rate(node_cpu_seconds_total{mode=~".*irq",instance="$node"}[$__rate_interval])) * 100
   ```

   **Query E: Busy Other**  
   ```promql
   sum by (rate(node_cpu_seconds_total{mode!='idle',mode!='user',mode!='system',mode!='iowait',mode!='irq',mode!='softirq',instance="$node"}[$__rate_interval])) * 100
   ```

   **Query F: Idle**  
   ```promql
   sum by (mode)(rate(node_cpu_seconds_total{mode='idle',instance="$node"}[$__rate_interval])) * 100
   ```

4. **Gráfico Memoria** (Time Series)
   **Query A: Ram total**  
   ```promql
   node_memory_MemTotal_bytes{instance="$node"}
   ```

   **Query B: Ram Used** 
   ```promql
   node_memory_MemTotal_bytes{instance="$node"} - node_memory_MemFree_bytes{instance="$node"} - (node_memory_Cached_bytes{instance="$node"} + node_memory_Buffers_bytes{instance="$node"})
   ```

   **Query C: Ram Cache + Buffer** 
   ```promql
   node_memory_Cached_bytes{instance="$node"} + node_memory_Buffers_bytes{instance="$node"}
   ```

   **Query D: Ram free** 
   ```promql
   node_memory_MemFree_bytes{instance="$node"}
   ```

   **Query E: SWAP Used** 
   ```promql
   (node_memory_SwapTotal_bytes{instance="$node"} - node_memory_SwapFree_bytes{instance="$node"})
   ```
---

### Añadir dashboards de la comunidad

Grafana permite importar dashboards preconfigurados desde la comunidad oficial:

Visita https://grafana.com/grafana/dashboards

Busca un dashboard que quieras usar y copia su ID.

En Grafana, ve a Dashboards > Import.

Selecciona Import via grafana.com y pega el ID.

Asigna el datasource correspondiente y haz clic en Import.

Esto facilita reutilizar dashboards ya probados y optimizados sin necesidad de crearlos desde cero.

### JSON

> Nota: El dashboard contiene variables y datasources que pueden necesitar ajustes según tu entorno:
> - Variable: `$node` → Selecciona el host que deseas monitorear.
> - Datasource: Asegúrate de que exista un datasource de tipo `Prometheus` y ajusta el UID si es necesario.

```json
{
  "annotations": {
    "list": [
      {
        "builtIn": 1,
        "datasource": {
          "type": "grafana",
          "uid": "-- Grafana --"
        },
        "enable": true,
        "hide": true,
        "iconColor": "rgba(0, 211, 255, 1)",
        "name": "Annotations & Alerts",
        "type": "dashboard"
      }
    ]
  },
  "editable": true,
  "fiscalYearStartMonth": 0,
  "graphTooltip": 0,
  "id": 2,
  "links": [],
  "panels": [
    {
      "datasource": {
        "type": "prometheus",
        "uid": "bexvhdg8riznke"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "mappings": [],
          "max": 100,
          "min": 0,
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "semi-dark-green",
                "value": 0
              },
              {
                "color": "yellow",
                "value": 65
              },
              {
                "color": "dark-red",
                "value": 80
              }
            ]
          },
          "unit": "percent"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 8,
        "w": 12,
        "x": 0,
        "y": 0
      },
      "id": 1,
      "options": {
        "minVizHeight": 75,
        "minVizWidth": 75,
        "orientation": "auto",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showThresholdLabels": false,
        "showThresholdMarkers": true,
        "sizing": "auto"
      },
      "pluginVersion": "12.1.1",
      "targets": [
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "(((count(count(node_cpu_seconds_total{instance=\"$node\"}) by (cpu))) - avg(sum by (mode)(rate(node_cpu_seconds_total{mode='idle',instance=\"$node\"}[$__rate_interval])))) * 100) / count(count(node_cpu_seconds_total{instance=\"$node\"}) by (cpu))",
          "legendFormat": "__auto",
          "range": true,
          "refId": "A"
        }
      ],
      "title": "CPU Usage",
      "type": "gauge"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "bexvhdg8riznke"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "thresholds"
          },
          "mappings": [],
          "max": 100,
          "min": 0,
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "semi-dark-green",
                "value": 0
              },
              {
                "color": "yellow",
                "value": 65
              },
              {
                "color": "dark-red",
                "value": 80
              }
            ]
          },
          "unit": "percent"
        },
        "overrides": []
      },
      "gridPos": {
        "h": 8,
        "w": 12,
        "x": 12,
        "y": 0
      },
      "id": 2,
      "options": {
        "minVizHeight": 75,
        "minVizWidth": 75,
        "orientation": "auto",
        "reduceOptions": {
          "calcs": [
            "lastNotNull"
          ],
          "fields": "",
          "values": false
        },
        "showThresholdLabels": false,
        "showThresholdMarkers": true,
        "sizing": "auto"
      },
      "pluginVersion": "12.1.1",
      "targets": [
        {
          "editorMode": "code",
          "expr": "100 - ((node_memory_MemAvailable_bytes{instance=\"$node\"} * 100) / node_memory_MemTotal_bytes{instance=\"$node\"})",
          "legendFormat": "__auto",
          "range": true,
          "refId": "A"
        }
      ],
      "title": "RAM Usage",
      "type": "gauge"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "bexvhdg8riznke"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisBorderShow": false,
            "axisCenteredZero": false,
            "axisColorMode": "text",
            "axisLabel": "",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "barWidthFactor": 0.6,
            "drawStyle": "line",
            "fillOpacity": 0,
            "gradientMode": "none",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "insertNulls": false,
            "lineInterpolation": "linear",
            "lineWidth": 1,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "auto",
            "spanNulls": false,
            "stacking": {
              "group": "A",
              "mode": "none"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "mappings": [],
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": 0
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          }
        },
        "overrides": [
          {
            "__systemRef": "hideSeriesFrom",
            "matcher": {
              "id": "byNames",
              "options": {
                "mode": "exclude",
                "names": [
                  "Busy system"
                ],
                "prefix": "All except:",
                "readOnly": true
              }
            },
            "properties": [
              {
                "id": "custom.hideFrom",
                "value": {
                  "legend": false,
                  "tooltip": false,
                  "viz": true
                }
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 8,
        "w": 12,
        "x": 0,
        "y": 8
      },
      "id": 3,
      "options": {
        "legend": {
          "calcs": [],
          "displayMode": "list",
          "placement": "bottom",
          "showLegend": true
        },
        "tooltip": {
          "hideZeros": false,
          "mode": "single",
          "sort": "none"
        }
      },
      "pluginVersion": "12.1.1",
      "targets": [
        {
          "editorMode": "code",
          "expr": "sum by (instance)(rate(node_cpu_seconds_total{mode=\"system\",instance=\"$node\"}[$__rate_interval])) * 100",
          "legendFormat": "Busy system",
          "range": true,
          "refId": "A"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "sum by (instance)(rate(node_cpu_seconds_total{mode='user',instance=\"$node\"}[$__rate_interval])) * 100",
          "hide": false,
          "instant": false,
          "legendFormat": "Busy User",
          "range": true,
          "refId": "B"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "sum by (instance)(rate(node_cpu_seconds_total{mode='iowait',instance=\"$node\"}[$__rate_interval])) * 100",
          "hide": false,
          "instant": false,
          "legendFormat": "Busy Iowait",
          "range": true,
          "refId": "C"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "sum by (instance)(rate(node_cpu_seconds_total{mode=~\".*irq\",instance=\"$node\"}[$__rate_interval])) * 100",
          "hide": false,
          "instant": false,
          "legendFormat": "Busy IRQs",
          "range": true,
          "refId": "D"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "sum by (instance) (\r\n  rate(node_cpu_seconds_total{mode!=\"idle\", instance=\"$node\"}[$__rate_interval])\r\n) * 100",
          "hide": false,
          "instant": false,
          "legendFormat": "Busy Other",
          "range": true,
          "refId": "E"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "sum by (mode)(rate(node_cpu_seconds_total{mode='idle',instance=\"$node\"}[$__rate_interval])) * 100",
          "hide": false,
          "instant": false,
          "legendFormat": "Idle",
          "range": true,
          "refId": "F"
        }
      ],
      "title": "CPU ",
      "type": "timeseries"
    },
    {
      "datasource": {
        "type": "prometheus",
        "uid": "bexvhdg8riznke"
      },
      "fieldConfig": {
        "defaults": {
          "color": {
            "mode": "palette-classic"
          },
          "custom": {
            "axisBorderShow": false,
            "axisCenteredZero": false,
            "axisColorMode": "text",
            "axisLabel": "",
            "axisPlacement": "auto",
            "barAlignment": 0,
            "barWidthFactor": 0.6,
            "drawStyle": "line",
            "fillOpacity": 0,
            "gradientMode": "none",
            "hideFrom": {
              "legend": false,
              "tooltip": false,
              "viz": false
            },
            "insertNulls": false,
            "lineInterpolation": "linear",
            "lineWidth": 1,
            "pointSize": 5,
            "scaleDistribution": {
              "type": "linear"
            },
            "showPoints": "auto",
            "spanNulls": false,
            "stacking": {
              "group": "A",
              "mode": "none"
            },
            "thresholdsStyle": {
              "mode": "off"
            }
          },
          "mappings": [],
          "max": 16000000000,
          "min": 0,
          "thresholds": {
            "mode": "absolute",
            "steps": [
              {
                "color": "green",
                "value": 0
              },
              {
                "color": "red",
                "value": 80
              }
            ]
          },
          "unit": "binbps"
        },
        "overrides": [
          {
            "__systemRef": "hideSeriesFrom",
            "matcher": {
              "id": "byNames",
              "options": {
                "mode": "exclude",
                "names": [
                  "Ram Used"
                ],
                "prefix": "All except:",
                "readOnly": true
              }
            },
            "properties": [
              {
                "id": "custom.hideFrom",
                "value": {
                  "legend": false,
                  "tooltip": false,
                  "viz": true
                }
              }
            ]
          }
        ]
      },
      "gridPos": {
        "h": 8,
        "w": 12,
        "x": 12,
        "y": 8
      },
      "id": 4,
      "options": {
        "legend": {
          "calcs": [],
          "displayMode": "list",
          "placement": "bottom",
          "showLegend": true
        },
        "tooltip": {
          "hideZeros": false,
          "mode": "single",
          "sort": "none"
        }
      },
      "pluginVersion": "12.1.1",
      "targets": [
        {
          "editorMode": "code",
          "expr": "node_memory_MemTotal_bytes{instance=\"$node\"}",
          "legendFormat": "Ram total",
          "range": true,
          "refId": "A"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "node_memory_MemTotal_bytes{instance=\"$node\"} - node_memory_MemFree_bytes{instance=\"$node\"} - (node_memory_Cached_bytes{instance=\"$node\"} + node_memory_Buffers_bytes{instance=\"$node\"})",
          "hide": false,
          "instant": false,
          "legendFormat": "Ram Used",
          "range": true,
          "refId": "B"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "node_memory_Cached_bytes{instance=\"$node\"} + node_memory_Buffers_bytes{instance=\"$node\"}",
          "hide": false,
          "instant": false,
          "legendFormat": "RAM Cache + Buffer",
          "range": true,
          "refId": "C"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "node_memory_MemFree_bytes{instance=\"$node\"}",
          "hide": false,
          "instant": false,
          "legendFormat": "Ram free",
          "range": true,
          "refId": "D"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "(node_memory_SwapTotal_bytes{instance=\"$node\"} - node_memory_SwapFree_bytes{instance=\"$node\"})",
          "hide": false,
          "instant": false,
          "legendFormat": "SWAP Used",
          "range": true,
          "refId": "E"
        },
        {
          "datasource": {
            "type": "prometheus",
            "uid": "bexvhdg8riznke"
          },
          "editorMode": "code",
          "expr": "node_memory_MemTotal_bytes{instance=\"localhost:9100\"} - node_memory_MemFree_bytes{instance=\"localhost:9100\"} - (node_memory_Cached_bytes{instance=\"localhost:9100\"} + node_memory_Buffers_bytes{instance=\"localhost:9100\"})",
          "hide": true,
          "instant": false,
          "legendFormat": "Uso de RAM",
          "range": true,
          "refId": "Alert-Q1"
        }
      ],
      "title": "Memory RAM",
      "type": "timeseries"
    }
  ],
  "preload": false,
  "schemaVersion": 41,
  "tags": [],
  "templating": {
    "list": [
      {
        "current": {
          "text": "localhost:9100",
          "value": "localhost:9100"
        },
        "definition": "label_values(node_uname_info{instance=\"localhost:9100\"},instance)",
        "label": "Host",
        "name": "node",
        "options": [],
        "query": {
          "qryType": 1,
          "query": "label_values(node_uname_info{instance=\"localhost:9100\"},instance)",
          "refId": "PrometheusVariableQueryEditor-VariableQuery"
        },
        "refresh": 1,
        "regex": "",
        "sort": 1,
        "type": "query"
      }
    ]
  },
  "time": {
    "from": "2025-09-12T12:30:42.832Z",
    "to": "2025-09-12T18:30:42.832Z"
  },
  "timepicker": {},
  "timezone": "browser",
  "title": "Node Exporter Personalizado 1",
  "uid": "d25878ac-007e-441f-bc50-e62440ff9eec",
  "version": 13
}
```