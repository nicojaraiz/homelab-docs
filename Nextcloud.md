# Nextcloud

[← Volver al inicio](./README.md)

## Qué es

Nextcloud es una plataforma de nube personal, pensada como alternativa autoalojada a servicios como Google Drive o Dropbox. Permite guardar archivos, sincronizarlos entre dispositivos y acceder a ellos desde cualquier lugar, pero almacenando todo en tu propia infraestructura. Además de archivos, Nextcloud funciona como un ecosistema ampliable mediante aplicaciones (plugins), que añaden funciones como notas, calendario, contactos, gestión de tareas y muchas más.

## Por qué lo uso

Mi misión con este servicio es reemplazar completamente a Google Drive. Es el servicio en el que estoy trabajando actualmente dentro del homelab.

Como servidor de ficheros ya funciona correctamente. Todavía tengo que darle un buen vistazo a todas las posibilidades que ofrece Nextcloud, ya que es una plataforma con muchísimas funciones más allá del simple almacenamiento de archivos.

## Estado actual

> [!NOTE]
> **Servicio en construcción:** Nextcloud es, a día de hoy, el servicio en el que estoy trabajando de forma más activa. La parte de almacenamiento de ficheros ya está operativa, pero quedan muchas funciones por explorar y configurar.

### Funciones activas

- **Almacenamiento de ficheros**: operativo, funcionando como servidor de archivos.
- **Notas**: instalado recientemente el plugin de notas, con el objetivo de tener la aplicación en el móvil para apuntar tareas y pendientes.

### Pendiente de explorar

- Resto de aplicaciones/plugins disponibles en el ecosistema Nextcloud.

### Próximos pasos

- Crear un usuario individual para cada miembro de la familia, igual que en Immich. De momento Nextcloud lo uso únicamente yo.

| Campo | Detalle |
|---|---|
| Servicio | Nextcloud |
| Función | Nube personal de archivos (sustituto de Google Drive) |
| Dónde corre | LXC 102 (Proxmox) |
| Almacenamiento | NAS (UNAS-4), vía NFS |
| Plugins instalados | Notas |
| Usuarios | Uno (yo), con previsión de añadir uno por miembro de la familia |
| Estado | En uso activo / en desarrollo |

---

## Conceptos relacionados

- [Self-hosting](./Conceptos.md#self-hosting)
- [Contenedor (LXC)](./Conceptos.md#contenedor-lxc)
- [NFS](./Conceptos.md#nfs)
- [NAS](./Conceptos.md#nas)