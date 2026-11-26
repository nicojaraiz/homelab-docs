# Red

[← Volver al inicio](./README.md)

## Direccionamiento IP

La red local utiliza el rango **192.168.1.0/24**.

El DHCP del router asigna las IPs de forma automática a todos los dispositivos, excepto a los siguientes, que tienen IP fija:

| Dispositivo / Servicio | IP | Asignación |
|---|---|---|
| Raspberry Pi | 192.168.1.5 | Fija |
| NAS (UNAS-4) | 192.168.1.6 | Fija |
| Pi-hole (LXC 100) | 192.168.1.100 | Fija |
| Immich (LXC 101) | 192.168.1.101 | Fija |
| Nextcloud (LXC 102) | 192.168.1.102 | Fija |
| PC de sobremesa | 192.168.1.47 | Fija |

> [!NOTE]
> **DNS de la red:** En la configuración del DHCP del router, el primer servidor DNS que se asigna a los dispositivos es el **192.168.1.100 (Pi-hole)**. De esta forma, todo el tráfico de resolución de nombres de la red pasa primero por Pi-hole, lo que permite el filtrado de anuncios y rastreadores a nivel de red completo.

---

## Puertos y accesos habituales

| Servicio / Equipo | Puerto / Acceso habitual | Uso |
|---|---|---|
| Proxmox VE (Raspberry Pi) | `8006` (`https://192.168.1.5:8006`) | Interfaz web de gestión de LXC y recursos |
| SSH (Raspberry Pi / UNAS-4) | `22` | Administración remota y apagado automático del UNAS-4 |
| Pi-hole | `80` / `53` | Panel web y resolución/filtrado DNS |
| Immich | `2283` | Interfaz web y sincronización móvil |
| Nextcloud | `80` / `443` | Nube personal de archivos y notas |

---

## Conceptos relacionados

- [Dirección IP / rango de red (notación CIDR)](./Conceptos.md#dirección-ip--rango-de-red-notación-cidr)
- [DHCP](./Conceptos.md#dhcp)
- [Servidor DNS](./Conceptos.md#servidor-dns)
- [VPN](./Conceptos.md#vpn)
- [SSH (Secure Shell)](./Conceptos.md#ssh-secure-shell)