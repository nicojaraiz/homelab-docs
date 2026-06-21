# Red

[← Volver al índice](./README.md)

## Situación actual

Ahora mismo tengo el router WiFi 6 Go de Movistar, con 1Gb de velocidad contratada por Ethernet.

A nivel de switch, tengo un switch GbE (Gigabit Ethernet), que me genera un pequeño cuello de botella respecto a la capacidad de velocidad que tiene el NAS, que es de 2.5GbE. Es decir, el NAS podría transferir datos más rápido de lo que el switch actual permite. No es ideal, pero con la capacidad económica que tengo ahora mismo, tampoco puedo hacer mucho más al respecto.

| Campo | Detalle |
|---|---|
| Router | WiFi 6 Go (Movistar) |
| Velocidad contratada | 1Gb (Ethernet) |
| Switch | GbE (1 Gbps) |
| Cuello de botella conocido | El switch limita a 1Gbps el tráfico hacia el NAS, que soporta 2.5GbE |

---

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
> **DNS de la red.** En la configuración del DHCP del router, el primer servidor DNS que se asigna a los dispositivos es el 192.168.1.100 (Pi-hole). De esta forma, todo el tráfico de resolución de nombres de la red pasa primero por Pi-hole, lo que permite el filtrado de anuncios y rastreadores a nivel de red completo.

---

## Plan a futuro: migración a Ubiquiti

Mi idea a futuro es migrar a un router de Ubiquiti. Como es una casa que comparto con mi familia, no quiero montar una infraestructura de red costosa ni voluminosa, así que estoy buscando equipos de tamaño desktop, ya que el espacio disponible es limitado.

Por eso, el candidato que estoy valorando es el **UniFi Express 7 (UX7)**, que es el router más sencillo dentro de la gama UniFi y encaja bien con una situación como la mía: hogar, espacio reducido, sin necesidad de una infraestructura de nivel empresarial.

| Campo | Detalle |
|---|---|
| Candidato | UniFi Express 7 (UX7) |
| Forma factor | Compacto, tipo desktop (117 x 117 x 42.5 mm) |
| Puerto WAN | 10 GbE RJ45 |
| Puerto LAN | 2.5 GbE RJ45 |
| Wi-Fi | Wi-Fi 7, tribanda (2.4/5/6 GHz) |
| Motivo de elección | Router de entrada a la gama UniFi, compacto y asequible |

> [!NOTE]
> Esto resolvería solo parte del cuello de botella. El UX7 tiene puerto LAN de 2.5GbE, lo cual igualaría la velocidad del router con la del NAS. Sin embargo, el switch actual (GbE) seguiría siendo una limitación a menos que también se sustituya por uno con puertos de 2.5GbE o superior.

> [!NOTE]
> **Fecha estimada.** Sin fecha cerrada. La migración se realizará cuando haya un colchón económico suficiente, ya que la inversión reciente en el NAS ha sido alta y toca dejar pasar unos meses antes del siguiente gasto importante.

---

## VPN (PiVPN)

El acceso remoto a la red se gestiona mediante PiVPN, alojado en la Raspberry Pi. Más detalles sobre su configuración y uso en la página de [Raspberry Pi](./RaspberryPi.md).

---

## Resumen

| Elemento | Actual | Futuro (planeado) |
|---|---|---|
| Router | WiFi 6 Go (Movistar) | UniFi Express 7 |
| Switch | GbE | Pendiente de definir |
| Velocidad contratada | 1Gb | 1Gb |
| Rango de red | 192.168.1.0/24 | 192.168.1.0/24 |
| Servidor DNS primario | Pi-hole (192.168.1.100) | Pi-hole (192.168.1.100) |
| Acceso remoto | PiVPN | PiVPN |

---

### Conceptos relacionados

- [DHCP](./Conceptos.md#dhcp)
- [Servidor DNS](./Conceptos.md#servidor-dns)
- [VPN](./Conceptos.md#vpn)
- [Dirección IP / rango de red](./Conceptos.md#dirección-ip--rango-de-red-notación-cidr)
