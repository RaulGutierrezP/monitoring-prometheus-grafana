# Configuración de alertas por correo en Grafana

En este documento se explica cómo configurar alertas en Grafana para que se envíen por correo electrónico utilizando un canal de alertas (AlertChannel) y SMTP.

---

## 1. Crear un canal de alertas (AlertChannel)

Un **AlertChannel** es el medio que Grafana utiliza para enviar notificaciones cuando se cumple una alerta.

1. Accede a Grafana y ve a **Alerting > Notification channels**.
2. Haz clic en **Add channel**.
3. Configura los siguientes campos:
   - **Name:** Nombre descriptivo para el canal, por ejemplo `CorreoEquipo`.
   - **Type:** Selecciona `Email`.
   - **Addresses:** Correo(s) de destino separados por comas, por ejemplo `equipo@empresa.com`.
4. Guarda el canal.

> Este canal se utilizará más adelante en la configuración de las reglas de alerta.

---

## 2. Configurar el servicio SMTP en Grafana

Para que Grafana pueda enviar correos electrónicos, debes configurar correctamente el servicio SMTP en el fichero `grafana.ini`. Busca la sección `[smtp]` y ajusta los parámetros:

```ini
[smtp]
enabled = true
host = smtp.tu-servidor.com:587
user = tu_usuario
password = tu_contraseña
from_address = grafana@tu-dominio.com
from_name = Grafana
skip_verify = false
```

### Después de realizar los cambios, reinicia el servicio de Grafana:
```bash
sudo systemctl restart grafana-server
```