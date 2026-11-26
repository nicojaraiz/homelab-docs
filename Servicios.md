# Servicios

[← Volver al inicio](./README.md)

Aquí tienes el listado de todos los servicios que tengo desplegados en el homelab. Cada uno cuenta con su propia página, donde explico qué es, para qué lo uso y cómo está configurado.

| Servicio | Qué es (resumen) | Dónde corre | Estado |
|---|---|---|---|
| [Pi-hole](./Pi-hole.md) | Bloqueador de publicidad y rastreadores a nivel de red | LXC 100 (Proxmox) | En uso |
| [Immich](./Immich.md) | Gestor y backup de fotos y vídeos, alternativa a Google Fotos | LXC 101 (Proxmox) | En uso |
| [Nextcloud](./Nextcloud.md) | Nube personal para archivos, alternativa a Google Drive/Dropbox | LXC 102 (Proxmox) | En uso |

## Infraestructura y soporte en la Raspberry Pi

| Componente | Función | Enlace |
|---|---|---|
| PiVPN | Acceso remoto seguro a la red local mediante VPN | [Raspberry Pi](./RaspberryPi.md) |
| NUT (`nutdrv_qx` + `upsmon`) | Monitorización del SAI Salicru y apagado automático del UNAS-4 y la Pi | [SAI / UPS](./SAI-UPS.md) |

---

## Notas generales

- Todos los servicios actuales de usuario corren como contenedores LXC dentro de Proxmox, en la Raspberry Pi.
- Esta tabla se irá ampliando a medida que se desplieguen nuevos servicios.