# Conceptos

Esta página es un glosario. Aquí explico, de la forma más simple posible, los términos técnicos que van apareciendo a lo largo de la documentación. La idea es que si no tienes ni idea del tema, puedas venir aquí, entender el concepto, y volver a la página donde lo viste sin perderte.

---

## Self-hosting

Consiste en alojar tú mismo tus propios servicios (correo, fotos, archivos, etc.) en hardware que controlas, en lugar de depender de empresas como Google, Apple o Dropbox. La ventaja es el control total sobre tus datos; la desventaja es que el mantenimiento, las copias de seguridad y la seguridad dependen de ti.

## Homelab

Un homelab es, literalmente, un "laboratorio casero": un conjunto de equipos (servidores, NAS, red) que una persona monta en su casa para aprender, experimentar y, normalmente, hacer self-hosting de sus propios servicios.

## RAID

RAID es una forma de combinar varios discos para conseguir más velocidad, más seguridad ante fallos, o ambas cosas. Hay distintos "niveles" de RAID, y no todos hacen lo mismo:

- **RAID 1 (espejo/mirror)**: los datos se copian de forma idéntica en dos discos. Si uno falla, el otro tiene exactamente la misma información. Es simple pero "gasta" la mitad del espacio total (dos discos de 1TB en RAID 1 dan 1TB usable).
- **RAID 5**: los datos y la información de redundancia (paridad) se reparten entre todos los discos del conjunto. Permite perder **un** disco sin perder datos, y aprovecha mejor el espacio que el RAID 1.
- **RAID 6**: similar al RAID 5, pero permite perder **dos** discos a la vez sin perder datos. A cambio, se pierde algo más de espacio usable.

> [!note] Relacionado
> Ver [[Hardware]] para la configuración RAID concreta usada en este homelab (RAID 1 en la Raspberry, RAID 5 en el NAS).

## NAS

Siglas de *Network Attached Storage* (almacenamiento conectado en red). Es un dispositivo dedicado a guardar archivos y ponerlos disponibles para el resto de equipos de la red, normalmente con varios discos en su interior configurados en RAID para evitar pérdida de datos.

## NFS

Siglas de *Network File System*. Es un protocolo que permite que un equipo (por ejemplo, la Raspberry Pi) acceda a carpetas de otro equipo (el NAS) como si fueran carpetas locales, a través de la red. Es una de las formas más comunes de compartir almacenamiento entre servidores Linux.

## SMB

Otro protocolo para compartir archivos en red, muy usado en entornos Windows (también compatible con macOS y Linux). Cumple una función similar a NFS, pero con otro funcionamiento por debajo.

## Hipervisor

Un hipervisor es un software que permite crear y gestionar máquinas virtuales y/o contenedores dentro de un mismo equipo físico, repartiendo los recursos (CPU, RAM, disco) entre ellos. Proxmox VE es un ejemplo de hipervisor.

## Contenedor (LXC)

Un contenedor es una forma ligera de aislar una aplicación o servicio, de modo que funcione de forma independiente del resto del sistema, pero sin necesitar los recursos de una máquina virtual completa. LXC es un tipo concreto de contenedor (el que usa Proxmox), distinto de Docker, aunque cumplen una función parecida: empaquetar y aislar servicios.

## Máquina virtual (VM)

Una máquina virtual es un "ordenador dentro de otro ordenador": un sistema operativo completo que se ejecuta de forma aislada sobre el hardware físico, gestionado por un hipervisor. A diferencia de un contenedor, una VM simula un equipo completo, lo que la hace más pesada pero también más aislada.

## Servidor DNS

DNS son las siglas de *Domain Name System*. Es el sistema que traduce nombres de dominio (como `google.com`) a direcciones IP, que es lo que realmente usan los equipos para comunicarse entre sí. Un servidor DNS es el encargado de hacer esa traducción. Pi-hole, en este homelab, actúa como servidor DNS de la red, lo que le permite filtrar peticiones antes de que lleguen a cargarse.

## VPN

Siglas de *Virtual Private Network* (red privada virtual). Permite conectarte de forma segura a una red remota (en este caso, la red de casa) desde cualquier otro sitio, como si estuvieras físicamente conectado a ella. Se usa, por ejemplo, para acceder a los servicios del homelab estando fuera de casa.

## DHCP

Siglas de *Dynamic Host Configuration Protocol*. Es el sistema que asigna automáticamente una dirección IP a cada dispositivo que se conecta a la red, para que no haya que configurarla manualmente en cada uno. En este homelab, algunos dispositivos tienen IP fija en lugar de recibirla por DHCP (ver [[Red]]).

## Dirección IP / rango de red (notación CIDR)

Una dirección IP identifica a un dispositivo dentro de una red. La notación `192.168.1.0/24` indica un rango de red: el `/24` señala cuántas direcciones caben dentro de ese rango (en este caso, hasta 254 dispositivos), y todas las IPs de ese rango comparten los primeros números (`192.168.1.X`).

## SAI / UPS

Siglas de **Sistema de Alimentación Ininterrumpida** (en inglés, *Uninterruptible Power Supply*). Es un dispositivo con baterías internas al que se conectan los equipos críticos del homelab (en este caso, la Raspberry Pi y el NAS). Si se va la luz, el SAI sigue suministrando electricidad durante unos minutos, dando margen para que los equipos se apaguen de forma ordenada y evitando que un corte brusco corrompa los discos o los datos (ver [[SAI-UPS]]).

## NUT (Network UPS Tools)

Conjunto de herramientas de código abierto para Linux que permite monitorizar un SAI (por ejemplo, a través de un cable USB) y automatizar acciones cuando hay un corte eléctrico. En este homelab corre en la Raspberry Pi, lee el estado de la batería del SAI Salicru y, cuando queda poca carga, dispara el apagado automático tanto del [[NAS]] como de la propia [[RaspberryPi]].

## SSH (Secure Shell)

Protocolo que permite conectarse y ejecutar comandos de forma remota y cifrada en otro equipo Linux a través de la red (como si tuvieras su terminal delante). Además de usarse para administrar la Raspberry Pi o el NAS, aquí se utiliza con **autenticación por clave pública** para que la Raspberry pueda enviar la orden de apagado al UNAS-4 sin necesidad de escribir una contraseña manualmente.

## Demonio (Daemon)

En Linux, un demonio es un programa que se ejecuta de forma continua en segundo plano sin que tengas que abrirlo manualmente, encargándose de tareas del sistema (por ejemplo, vigilar el estado del SAI). Suelen gestionarse con `systemd` y sus nombres acostumbran a terminar en la letra `d` (como `upsd` o `sshd`).

---

> [!note] Pendiente de ampliar
> Esta página se irá ampliando con nuevos conceptos a medida que aparezcan en el resto de la documentación.