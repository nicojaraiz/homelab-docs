# Immich

## Qué es

Immich es una aplicación de gestión y backup de fotos y vídeos, pensada como alternativa autoalojada a servicios como Google Fotos o iCloud. Permite subir fotos desde el móvil de forma automática, organizarlas, buscarlas y verlas desde cualquier dispositivo, pero con la diferencia de que todo se guarda en tu propia infraestructura en lugar de en los servidores de una empresa.

Tiene aplicación para móvil (que hace backup automático del carrete, igual que harían Google Fotos o iCloud), interfaz web, y soporta múltiples usuarios, lo que permite que distintas personas tengan su propia cuenta y su propia biblioteca de fotos dentro del mismo servidor.

## Por qué lo uso

Lo uso para tener un sitio donde mi familia y yo podamos guardar nuestras fotos y vídeos sin depender de servicios de terceros, manteniendo el control total sobre dónde se almacenan esos datos.

## Configuración

Las fotos se guardan en el NAS (UNAS-4), no en el almacenamiento local de la Raspberry, aprovechando así la redundancia que ofrece el RAID 5 del NAS.

He creado un usuario individual para cada miembro de mi familia, de forma que cada uno tiene su propia biblioteca dentro de Immich.

### Backup automático

La aplicación móvil realiza cada día un escaneo de las imágenes del dispositivo. Todas las fotos y vídeos nuevos se suben automáticamente al backup de Immich, sin necesidad de hacerlo de forma manual.

### Usuarios y permisos

Cada usuario cuenta con 200GB de almacenamiento para fotos y vídeos, y tiene permisos para crear carpetas sin problemas.

| Campo | Detalle |
|---|---|
| Servicio | Immich |
| Función | Gestión y backup de fotos y vídeos |
| Dónde corre | LXC 101 (Proxmox) |
| Almacenamiento de fotos | NAS (UNAS-4), vía NFS |
| Usuarios | Uno por cada miembro de la familia |
| Cuota por usuario | 200GB |
| Backup automático | Sí, escaneo diario desde la app móvil |
| Estado | En uso |

---

## Conceptos relacionados

- [[Self-hosting]]
- [[Contenedor (LXC)]]
- [[NFS]]
- [[NAS]]