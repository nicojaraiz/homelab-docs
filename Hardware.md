# Hardware

[← Volver al inicio](./README.md)

Vamos a repasar con qué cuento físicamente para este proyecto. Como ya he adelantado, soy bastante agarrado con el dinero, así que aquí no vais a encontrar un datacenter, pero sí lo justo y necesario para que todo funcione bien.

---

## Resumen

| Equipo | Rol | Specs clave | Estado |
|---|---|---|---|
| Raspberry Pi 5 | Servidor principal / nodo NUT | 8GB RAM, RAID 1 (mirror) en NVMe, Raspberry Pi OS | En uso |
| UNAS-4 (Ubiquiti) | Almacenamiento (NAS) | 4 bahías HDD, 8TB brutos / 4TB usables, RAID 5 | En uso |
| SAI Salicru | Continuidad eléctrica y apagado seguro | Conexión USB a Raspberry Pi, protocolo Qx (`nutdrv_qx`) | En uso |
| PC de sobremesa | Estación de trabajo | — | En uso |

---

## Raspberry Pi 5

Este es el cerebro de todo el proyecto.

Es una Raspberry Pi 5 con 8GB de RAM, que creo que serán suficientes para mis ideas. Como tener todo en la tarjeta SD me daba miedito (cualquiera que haya perdido una SD de golpe me entiende), le metí un HAT con dos ranuras NVMe M.2, para así montar un RAID en espejo (RAID 1) y tener redundancia de datos en todo momento.

Aviso: tengo pánico a la pérdida de datos. Por eso este RAID me parecía indispensable antes de meterme en cualquier otra cosa.

| Campo | Detalle |
|---|---|
| Modelo | Raspberry Pi 5 |
| RAM | 8GB |
| Sistema operativo | Raspberry Pi OS |
| Almacenamiento | 2x NVMe M.2 (vía HAT) en RAID 1 |
| Rol | Servidor principal / nodo maestro NUT |
| Fecha de adquisición | Pendiente |

> [!NOTE]
> **Pendiente de documentar:** Modelo exacto del HAT y capacidad de los NVMe.

---

## UNAS-4 (Ubiquiti)

Madre mía, cómo me dolió darle a comprar a este bicho.

Lo adquirí hace un par de semanas (junio 2026). Es un NAS de la marca Ubiquiti con 4 bahías para discos HDD, que compré de segunda mano en Wallapop a un italiano. Con esto tenemos 8TB de discos en bruto, 4TB usables, ya que lógicamente busco máxima redundancia de datos.

Al ser de segunda mano no espero mucha vida útil, pero para dar paso a mis ideas necesitaba un lugar donde tener mis datos de manera segura. Y de paso, qué mejor manera de meterme en el ecosistema Ubiquiti que con su propio NAS.

| Campo | Detalle |
|---|---|
| Modelo | UNAS-4 (Ubiquiti) |
| Bahías | 4x HDD |
| Capacidad bruta | 8TB |
| Capacidad usable | 4TB |
| Configuración RAID | RAID 5 |
| Origen | Segunda mano (Wallapop) |
| Fecha de adquisición | Junio 2026 |
| Rol | Almacenamiento principal / backups |

> [!WARNING]
> **Hardware de segunda mano:** Al no ser nuevo, no se espera una vida útil muy larga. Vigilar estado de los discos y salud general del equipo.

---

## SAI Salicru

Incorporado para evitar sustos durante tormentas y cortes de luz. Se conecta por USB a la Raspberry Pi 5 para que NUT supervise el estado eléctrico y dispare el apagado limpio del UNAS-4 y de la propia Raspberry ante un corte prolongado (ver [SAI / UPS](./SAI-UPS.md)).

| Campo | Detalle |
|---|---|
| Marca | Salicru |
| Conexión al host | USB (ID `0665:5161`) |
| Gestión | NUT (`nutdrv_qx` + `upsmon`) sobre Raspberry Pi |
| Rol | Continuidad eléctrica y apagado ordenado |

---

## PC de sobremesa

A nivel homelab no tengo nada más. Mi única forma de trabajar en este proyecto es desde mi PC de sobremesa, ya que no tengo portátil.

No trabajo de forma constante: voy un poco a sprints. Durante una semana puedo estar como un loco metido en el proyecto, y después lo dejo durante un mes entero. No tengo un ritmo fijo porque quiero hacer esto cuando sienta ganas, y no quiero sentirme anclado a él.

| Campo | Detalle |
|---|---|
| Rol | Estación de trabajo / acceso al homelab |
| Uso | Configuración, mantenimiento, acceso a servicios |

---

## Notas generales

- Filosofía de hardware: lo justo y necesario, priorizando siempre la redundancia de datos antes que la expansión de servicios.
- Ritmo de trabajo: por sprints, sin constancia fija.