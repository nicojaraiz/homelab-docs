# Procedimientos

Esta página reúne procedimientos paso a paso para tareas de mantenimiento y gestión del homelab. La idea es no depender de la memoria ni tener que rebuscar en el historial del navegador cada vez que necesito hacer algo que ya he hecho antes.

Cada procedimiento sigue, en la medida de lo posible, esta estructura:

- **Cuándo se usa**: en qué situación aplica este procedimiento.
- **Requisitos previos**: qué hace falta antes de empezar (accesos, backups, etc.).
- **Pasos**: la lista numerada de acciones a realizar.
- **Verificación**: cómo comprobar que ha salido bien.
- **Notas**: problemas conocidos, advertencias, o cosas a tener en cuenta.

---

## Proxmox

- Añadir un nuevo contenedor LXC
- Asignar/modificar recursos de un contenedor existente
- Backup y restauración de un contenedor

## NAS

- Qué hacer si falla un disco
- Añadir una nueva unidad/carpeta compartida por NFS
- Comprobar el estado del RAID

## SAI / NUT

- Comprobar estado y carga del SAI Salicru (`upsc salicru@localhost`)
- Verificar el servicio `nut-monitor` y la conexión SSH con el UNAS-4
- Probar el apagado ordenado del UNAS-4 y la Raspberry Pi

## Pi-hole

- Añadir un dominio a la blocklist
- Añadir/actualizar una lista de bloqueo
- Restaurar configuración tras un fallo

## Immich

- Restaurar un backup
- Añadir un nuevo usuario

## Nextcloud

- Restaurar un backup
- Añadir un nuevo usuario
- Instalar/actualizar un plugin

## Red

- Reservar una IP fija para un nuevo dispositivo
- Acceder al homelab por VPN desde fuera de casa

---

> [!note] Pendiente
> La estructura está preparada; iré redactando cada procedimiento a medida que lo ejecute o revise en el homelab.