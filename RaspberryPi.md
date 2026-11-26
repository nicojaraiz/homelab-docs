# RaspberryPi

La Raspberry tiene tres funciones esenciales dentro del proyecto: acceso remoto mediante VPN, virtualización ligera con Proxmox VE y gestión energética con NUT.

## VPN con PiVPN

La primera de ellas es una VPN, la cual gestiono con PiVPN, que según he visto es la más adaptada a una Raspberry y la más fácil de configurar. No me he complicado mucho la vida, así que tengo esto montado de forma sencilla.

¿Qué uso le doy? Básicamente tener mi móvil siempre conectado a casa, y cuando tenía portátil, poder trabajar de vez en cuando en el proyecto estando fuera. Nada súper especial, pero cumple su función.

| Campo | Detalle |
|---|---|
| Herramienta | PiVPN |
| Función | Acceso remoto a la red local |
| Uso principal | Móvil conectado de forma permanente; acceso ocasional desde fuera de casa |

---

## Proxmox VE

Y aquí viene la parte que más me gusta de la Raspberry.

La Raspberry es un nodo de Proxmox VE. Sí, la Raspberry aloja un servidor Proxmox, y la verdad es que está muy chulo. De esta manera puedo tener monitorización de recursos y un control mediante contenedores. Hoy en día Docker es el líder, creo yo; no tengo datos exactos, pero es de lo que más he escuchado hablar. Vi que se podía meter Proxmox en una Raspberry y dije: venga, para adelante. Aquí el miedo hay que dejarlo a un lado y centrarse en aprender a base de errores.

Así que, dicho esto, la Raspberry tiene Proxmox, desde donde controlo todos los contenedores virtuales en los que alojo mis servicios. Con Proxmox, de una manera muy visual, puedo asignar los recursos que quiera a cada contenedor de forma sencilla, además de poder gestionarlo todo desde una misma interfaz, lo cual a nivel de gestión lo facilita todo.

> [!note] Proxmox VE en Raspberry Pi: no es la versión oficial
> Proxmox VE está pensado oficialmente para procesadores x86 (Intel/AMD), no para ARM. Lo que corre en la Raspberry es un **port no oficial mantenido por la comunidad** (compilado para ARM64), que replica la funcionalidad de Proxmox sobre Raspberry Pi OS.
>
> Funciona muy bien para el uso que le voy a dar (contenedores LXC ligeros), pero conviene tenerlo presente: las plantillas de contenedores tienen que ser de arquitectura ARM64 (no sirven las x86), y no todas las máquinas virtuales van a funcionar igual de bien que en una instalación oficial.

| Campo | Detalle |
|---|---|
| Software | Proxmox VE (port comunitario para ARM64) |
| Versión | 8.3.2+port |
| Corre sobre | Raspberry Pi OS |
| Nodo | raspberrypi |
| IP de acceso | `192.168.1.5` (`https://192.168.1.5:8006`) |
| Función | Hipervisor / gestión de contenedores y VMs |
| Workloads principales | Contenedores LXC |
| Acceso | Interfaz web de Proxmox |

### Contenedores LXC actuales

| ID | Nombre | Memoria | Swap | Cores | Disco | Punto de montaje |
|---|---|---|---|---|---|---|
| 100 | pinet (Pi-hole) | 1.00 GiB | 1.00 GiB | 1 | 12G | — |
| 101 | immich | 2.00 GiB | 1.49 GiB | 2 | 20G | /mnt/unifi_immich → /mnt/photos |
| 102 | nextcloud | 2.00 GiB | 512 MiB | 2 | 20G | /mnt/nas-nextcloud → /mnt/nextcloud-data |

> [!note] Conexión con el NAS
> El UNAS-4 está conectado a la Raspberry mediante NFS. Los detalles de configuración (exports, permisos, etc.) se documentarán en la página dedicada al NAS.

---

## Gestión energética y apagado automático (NUT)

La Raspberry Pi actúa como nodo maestro conectado por USB al SAI Salicru mediante **NUT (Network UPS Tools)**. Cuando hay un corte de luz y se alcanza batería baja (`LOWBATT`), ejecuta un script que envía `poweroff` por SSH al UNAS-4 (`192.168.1.6`), espera 30 segundos y apaga la propia Raspberry Pi. Todos los detalles están en [[SAI-UPS]].

---

## Resumen

| Función | Herramienta | Para qué la uso |
|---|---|---|
| VPN | PiVPN | Acceso remoto al homelab desde el móvil u otros dispositivos |
| Virtualización | Proxmox VE (ARM) | Alojar y gestionar todos los contenedores y servicios |
| Continuidad eléctrica | NUT (`nutdrv_qx` + `upsmon`) | Supervisar el SAI Salicru y apagar el UNAS-4 y la Raspberry de forma segura |

---

## Conceptos relacionados

- [[Hipervisor]]
- [[Contenedor (LXC)]]
- [[VPN]]
- [[NFS]]
- [[NUT (Network UPS Tools)]]
- [[SAI / UPS]]