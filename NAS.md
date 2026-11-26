# UNAS-4 (NAS)

[← Volver al inicio](./README.md)

## Historia

Antes de tener este UNAS-4, contaba con un NAS D-Link muy antiguo de dos bahías. Sin embargo, no me sentía seguro guardando ahí datos importantes para mí, como las imágenes familiares, así que decidí dar el paso e invertir en un NAS mucho más potente y moderno: el que tengo actualmente.

## Filosofía de uso

Mi idea es, lógicamente, almacenar absolutamente todo en local y no depender de ningún servicio ni proveedor externo.

El tema de carpetas compartidas, permisos detallados y estructuras complejas siempre ha sido algo que, más que pereza, requiere una inversión de tiempo mucho mayor que simplemente instalar un Nextcloud y tenerlo todo centralizado ahí directamente. Así que esa ha sido mi aproximación: en lugar de montar una estructura compleja de carpetas compartidas en el NAS, dejo que sean los propios servicios (Nextcloud, Immich) los que organicen y gestionen sus datos, y el NAS simplemente les da el almacenamiento por debajo.

## Conexión con la Raspberry (NFS)

El NAS comparte el almacenamiento hacia la Raspberry mediante NFS. Actualmente tengo dos unidades compartidas, una para cada servicio:

| Unidad | Uso | Espacio usado | Storage Pool |
|---|---|---|---|
| immich | Almacenamiento de fotos/vídeos de Immich | 52.34 GB | Storage Pool 1 |
| nextcloud | Almacenamiento de archivos de Nextcloud | 159 MB | Storage Pool 1 |
| UniFi's Drive | Unidad propia del sistema UniFi | 65.5 KB | Storage Pool 1 |

### Configuración de la conexión NFS

| Campo | Detalle |
|---|---|
| IP del NAS | 192.168.1.6 |
| Modo de escritura NFS | Sync (las escrituras se confirman en disco antes de responder al cliente, mayor integridad de datos) |
| Permisos (immich) | Read-Write |
| Permisos (nextcloud) | Read-Write |
| Squash | Collaborative |

> [!NOTE]
> **¿Qué es el "squash" en NFS?** Es una configuración que determina cómo se traducen los permisos de usuario entre el cliente (la Raspberry) y el NAS. El modo "Collaborative" está pensado para que varios usuarios o servicios puedan escribir en la misma carpeta compartida sin conflictos de permisos.

## Apagado automático ante cortes de luz

El UNAS-4 no integra un cliente NUT genérico de terceros, por lo que su apagado ordenado ante un corte eléctrico se orquesta desde la Raspberry Pi mediante **SSH por clave pública** cuando el SAI Salicru llega a batería baja (ver [SAI / UPS](./SAI-UPS.md)).

## Resumen

| Campo | Detalle |
|---|---|
| Modelo | UNAS-4 (Ubiquiti) |
| IP | 192.168.1.6 |
| Protocolo de conexión | NFS |
| Unidades compartidas | immich, nextcloud, UniFi's Drive |
| Servicios conectados | Immich (LXC 101), Nextcloud (LXC 102) |
| Filosofía | Almacenamiento centralizado en local, sin estructuras complejas de carpetas; los servicios gestionan sus propios datos |

---

## Conceptos relacionados

- [NAS](./Conceptos.md#nas)
- [NFS](./Conceptos.md#nfs)
- [RAID](./Conceptos.md#raid)
- [SAI / UPS](./Conceptos.md#sai--ups)
- [SSH (Secure Shell)](./Conceptos.md#ssh-secure-shell)