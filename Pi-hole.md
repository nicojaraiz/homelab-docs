# Pi-hole

[← Volver a Servicios](./Servicios.md) · [Índice](./README.md)

## Qué es

Pi-hole es un servicio que actúa como filtro de publicidad y rastreadores para toda la red, en lugar de tener que instalar un bloqueador en cada dispositivo por separado. Funciona como un servidor DNS: cuando cualquier dispositivo de la red (móvil, ordenador, smart TV...) intenta cargar una página, esa petición pasa primero por Pi-hole, que comprueba si el dominio solicitado pertenece a una lista de anuncios, rastreadores o dominios maliciosos conocidos. Si es así, la bloquea antes de que llegue a cargarse.

La ventaja frente a un bloqueador de anuncios tradicional (como una extensión de navegador) es que protege toda la red de una vez: móviles, tablets, smart TVs, consolas... cualquier dispositivo conectado, sin tener que configurar nada en cada uno de ellos. Además, al bloquear estas peticiones antes de que se descarguen, también se reduce el tráfico de red.

## Por qué lo uso

Empecé a usarlo principalmente para no tener anuncios en casa y para aliviar bastante tráfico de red, al evitar que se carguen todos esos elementos de publicidad y rastreo de forma constante.

## Listas de bloqueo

Estuve unos meses usando la lista por defecto de Pi-hole, pero más adelante me pasé a una lista de la comunidad.

| Campo | Detalle |
|---|---|
| Lista | HaGeZi's Pro DNS Blocklist |
| Tipo | Adblock Plus (sintaxis AdBlock) |
| Qué bloquea | Anuncios, afiliados, rastreo, métricas, telemetría, phishing, malware, scam, contenido fake, cryptojacking, entre otros |
| Repositorio | https://github.com/hagezi/dns-blocklists |
| Entradas (a fecha de consulta) | 228.709 |

> [!NOTE]
> **Configuración DNS.** Pi-hole está configurado como servidor DNS de la red, pero no se utiliza como resolvedor de nombres (no gestiona la resolución de dominios en sí). Su función aquí se limita al filtrado mediante la lista de bloqueo, actuando efectivamente como un adblock a nivel de red.

## Resumen

| Campo | Detalle |
|---|---|
| Servicio | Pi-hole |
| Función | Bloqueo de anuncios y rastreadores a nivel de red |
| Dónde corre | LXC 100 (pinet) en Proxmox |
| Configurado como DNS de la red | Sí |
| Lista de bloqueo activa | HaGeZi's Pro DNS Blocklist |
| Estado | En uso |

---

### Conceptos relacionados

- [Servidor DNS](./Conceptos.md#servidor-dns)
