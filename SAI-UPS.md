# SAI / UPS (Salicru + NUT)

## ¿Por qué un SAI en el homelab?

Cuando empecé a montar todo el homelab (la Raspberry Pi con Proxmox, los contenedores y el UniFi UNAS-4 con todos los discos y copias), había algo que me tenía bastante intranquilo: **las tormentas y los microcortes de luz**.

Cada vez que empezaba a llover fuerte o veía bajar la intensidad de las luces, estaba pendiente por si se iba la corriente de golpe. Un apagón brusco en un NAS con discos en marcha o en un hipervisor con contenedores escribiendo datos es comprar papeletas para corromper el sistema de archivos o perder información. Esa no era manera de seguir, así que decidí comprar un **SAI (Sistema de Alimentación Ininterrumpida) Salicru** para proteger toda la infraestructura y automatizar un apagado limpio cuando haya un corte prolongado.

---

## Arquitectura del sistema

El **UniFi UNAS-4** no soporta de forma nativa clientes NUT de terceros (está pensado para emparejarse únicamente con el *UniFi UPS Tower* propietario). Para resolverlo sin complicarme, he convertido la **Raspberry Pi en el nodo maestro de energía** usando **NUT (Network UPS Tools)**:

1. El **SAI Salicru** se conecta por cable USB a la **Raspberry Pi 5**.
2. La Raspberry Pi lee en tiempo real el estado de la batería y la red eléctrica mediante el driver `nutdrv_qx`.
3. Si se va la luz y la batería llega a un nivel crítico (`LOWBATT`), la Raspberry Pi ejecuta un script de emergencia que **se conecta por SSH al UNAS-4 (`192.168.1.6`) para apagarlo de forma ordenada**, espera unos segundos a que desmonte volúmenes y, finalmente, **se apaga a sí misma**.

| Campo | Detalle |
|---|---|
| Hardware | SAI Salicru (USB ID `0665:5161` - Cypress Semiconductor) |
| Software en host | NUT (`nut-server` + `nut-monitor` 2.8.0) |
| Driver NUT | `nutdrv_qx` (protocolo Mustek / Qx) |
| Nodo maestro | Raspberry Pi 5 (`raspberrypi`, `192.168.1.5`) |
| Nodo apagado por SSH | UniFi UNAS-4 (`192.168.1.6`) |
| Autonomía estimada | ~15–25 minutos con una carga actual del ~11% |

> [!note] Sobre el porcentaje de batería
> Este modelo de Salicru no reporta el porcentaje exacto por firmware, sino que NUT lo estima a partir del voltaje de la batería (`13.50 V` en reposo hasta `10.40 V` descargada). Cuando el SAI entra en umbral de batería baja, emite la señal `LB` (`LOWBATT`) y dispara el apagado automático.

---

## Instalación y configuración en la Raspberry Pi

### 1. Verificación del USB y paquetes

Al conectar el cable USB del SAI a la Raspberry Pi, el kernel lo detecta con `lsusb`:

```bash
lsusb
# Bus 001 Device 012: ID 0665:5161 Cypress Semiconductor USB to Serial
```

Instalamos NUT en el sistema:

```bash
apt update && apt install nut -y
```

### 2. Archivos de configuración de NUT (qué cambiar en cada uno)

#### `/etc/nut/nut.conf`
Definimos el modo `standalone` para que la propia Raspberry Pi gestione el driver, el servidor y el monitor local:

```ini
MODE=standalone
```

#### `/etc/nut/ups.conf`
Declaramos el SAI utilizando el driver `nutdrv_qx` y fijando su `vendorid` y `productid`:

```ini
[salicru]
    driver = nutdrv_qx
    port = auto
    vendorid = 0665
    productid = 5161
    desc = "SAI Salicru"
```

#### `/etc/nut/upsd.users`
Creamos el usuario interno `monuser` que utilizará el demonio monitor (`upsmon`) para conectarse al servidor NUT local (sustituye `<PASSWORD_LOCAL_NUT>` por tu contraseña):

```ini
[monuser]
    password = <PASSWORD_LOCAL_NUT>
    upsmon master
```

> [!warning] Importante con la contraseña de NUT
> En los archivos `.conf` y `.users` de NUT, el carácter `#` se interpreta como el inicio de un comentario. La contraseña elegida **no debe contener `#`**, espacios ni comillas, o `upsmon` cortará la línea y fallará al arrancar. Además, debe coincidir exactamente con la puesta en `upsmon.conf`.

#### `/etc/nut/upsmon.conf`
Indicamos qué SAI debe vigilar `upsmon` y qué script debe lanzar cuando reciba la orden de apagado (sustituye `<PASSWORD_LOCAL_NUT>` por la misma contraseña definida en `upsd.users`):

```ini
MONITOR salicru@localhost 1 monuser <PASSWORD_LOCAL_NUT> master
SHUTDOWNCMD "/usr/local/bin/apagado-emergencia.sh"
```

#### `/etc/tmpfiles.d/nut.conf`
Para asegurar que el directorio `/run/nut` exista siempre con los permisos adecuados tras cada reinicio de la Raspberry Pi:

```ini
d /run/nut 0770 nut nut -
```

---

## Apagado remoto del UNAS-4 por SSH

Como el servicio `nut-monitor` ejecuta las acciones bajo el usuario del sistema `nut`, generamos un par de claves SSH exclusivo para ese usuario y copiamos la clave pública al UNAS-4 (`192.168.1.6`), habiendo activado previamente el acceso SSH en la consola de UniFi OS:

```bash
mkdir -p /var/lib/nut/.ssh
ssh-keygen -t ed25519 -f /var/lib/nut/.ssh/id_ed25519 -N ""
chown -R nut:nut /var/lib/nut/.ssh

# Copiar la clave pública al NAS
sudo -u nut ssh-copy-id -i /var/lib/nut/.ssh/id_ed25519.pub root@192.168.1.6
```

### Script de apagado (`/usr/local/bin/apagado-emergencia.sh`)

Este es el script que ejecuta `upsmon` en caso de emergencia (`LOWBATT`):

```bash
#!/bin/bash
UNAS_IP="192.168.1.6"
SSH_KEY="/var/lib/nut/.ssh/id_ed25519"

# 1. Mandar orden de apagado limpio al UniFi UNAS-4
ssh -i "$SSH_KEY" -o ConnectTimeout=5 -o StrictHostKeyChecking=no root@"$UNAS_IP" "poweroff"

# 2. Esperar 30 segundos para que el NAS cierre servicios y desmonte discos
sleep 30

# 3. Apagar la Raspberry Pi
/sbin/shutdown -h +0
```

Permisos de ejecución:

```bash
chmod +x /usr/local/bin/apagado-emergencia.sh
```

---

## Comandos útiles de comprobación

* **Ver estado y parámetros del SAI en tiempo real:**
  ```bash
  upsc salicru@localhost
  ```
  *(Valores habituales en `ups.status`: `OL` = conectado a la red eléctrica, `OB` = tirando de batería, `LB` = batería baja).*

* **Comprobar el estado del servicio monitor:**
  ```bash
  systemctl status nut-monitor --no-pager
  ```

---

## Conceptos relacionados

- [[SAI / UPS]]
- [[NUT (Network UPS Tools)]]
- [[SSH (Secure Shell)]]
- [[Demonio (Daemon)]]
- [[NFS]]