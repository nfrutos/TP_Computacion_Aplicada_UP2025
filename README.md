# Trabajo Práctico Integrador – Computación Aplicada  
Universidad de Palermo – 2025  

## Integrantes del grupo
- Mateo Bonomi  
- Giovanni Feruglio  
- Nahuel Matías Frutos  
- Santiago Valle  
- Candela Sánchez  

---

## 1. Configuración del entorno

- Sistema operativo: Debian GNU/Linux 11 (Bullseye)  
- Hostname configurado: **TPServer**  
- Acceso root habilitado mediante autenticación SSH con clave pública y privada.  
- Clave privada: `clave_privada.txt`  
- Clave pública: `clave_publica.pub`  
- Contraseña del usuario root: `palermo`

---

## 2. Servicios instalados y configurados

### SSH
- Servicio `ssh` instalado y habilitado.  
- Permite acceso remoto al usuario root mediante clave privada.  
- Conectividad confirmada desde la máquina física con el comando:  ssh -i ~/Descargas/computacionAplicada/clave_privada.txt root@172.20.10.8

### Servidor Web (Apache + PHP)
- Versión de Apache: **2.4.65**  
- Versión de PHP: **7.4**  
- Carpeta raíz configurada en: `/www_dir`  
- Archivos servidos: `index.php` y `logo.png`  
- El sitio web es accesible desde la red local en:  
**http://172.20.10.8**  
- Apache configurado para apuntar a `/www_dir` en el archivo `/etc/apache2/sites-available/000-default.conf`.

### Base de Datos (MariaDB)
- Motor: **MariaDB 10.5.29**  
- Base de datos creada: **ingenieria**  
- Usuario: **lcars**  
- Contraseña: **palermo**  
- Tablas cargadas desde el script `/mnt/shared/db.sql`:  
- `alumnos`  
- `modulos`  
- `notas`  
- Conectividad verificada mediante `index.php` y acceso exitoso al sitio.

---

## 3. Configuración de Red

- Interfaz configurada: **enp0s3**  
- Dirección IP estática: **172.20.10.8**  
- Máscara de subred: **255.255.255.0**  
- Gateway: **172.20.10.1**  
- Configuración persistente en el archivo `/etc/network/interfaces`.

---

## 4. Almacenamiento

- Disco adicional de 10 GB agregado y dividido en dos particiones:  
- `/www_dir` con 3 GB  
- `/backup_dir` con 6 GB  
- Ambas configuradas para montarse automáticamente al iniciar el sistema operativo.  
- Apache configurado para servir los archivos desde `/www_dir`.  
- El archivo `/root/particion` contiene una copia persistente del contenido de `/proc/partitions`, ya que el archivo original de `/proc` es efímero.

---

## 5. Script de Backup

**Ruta:** `/opt/scripts/backup_full.sh`

### Funcionalidad
- Recibe dos parámetros: **origen** y **destino**.  
- Verifica la disponibilidad de los sistemas de archivos antes de ejecutar el backup.  
- Crea archivos comprimidos `.tar.gz` con la fecha en formato ANSI (YYYYMMDD).  
- Incluye una opción de ayuda (-help) para guiar al usuario.

### Ejemplo de uso:

/opt/scripts/backup_full.sh /var/log /backup_dir

**Resultado esperado:**
Backup creado correctamente: /backup_dir/log_bkp_20251024.tar.gz


---

## 6. Tareas automáticas (cron)

Configuradas mediante `crontab -e`:

0 0 * * * /opt/scripts/backup_full.sh /var/log /backup_dir
0 23 * * 1,3,5 /opt/scripts/backup_full.sh /www_dir /backup_dir


- Todos los días a las 00:00 se backupea `/var/log`.  
- Lunes, miércoles y viernes a las 23:00 se backupea `/www_dir`.  

Verificación con `crontab -l` muestra ambas tareas correctamente instaladas.

---

## 7. Archivos comprimidos para entrega

Ubicados en el directorio:  
`~/Documentos/TP_Computacion_Aplicada/entrega_tp/`

| Archivo | Contenido |
|----------|-----------|
| `backup_dir.tar.gz` | Directorio de backups generados |
| `etc.tar.gz` | Configuraciones del sistema |
| `opt.tar.gz` | Archivos adicionales en `/opt` |
| `opt_scripts.tar.gz` | Script de backup ubicado en `/opt/scripts` |
| `proc.tar.gz` | Información general del sistema |
| `proc_part.tar.gz` | Contiene `/proc/partitions` |
| `root.tar.gz` | Archivos del usuario root |
| `var_part.tar.gzaa` y `var_part.tar.gzab` | Carpeta `/var` dividida en partes |
| `www_dir.tar.gz` | Archivos del sitio web (index.php y logo.png) |

---
## 8. Diagrama topológico

#A continuación se presenta el diagrama topológico de la infraestructura implementada, que muestra la relación entre el host físico, la red interna de VirtualBox, el servidor Debian y los servicios configurados.

![Diagrama Topológico](./Infografía%20-%20Diagrama%20Topológico.png)
> **Figura 1.** Diagrama topológico del entorno de red y servicios implementado (Apache, PHP, MariaDB, SSH y backup automático por cron).
---

## 9. Estado final

- Todos los servicios funcionan correctamente.  
- El servidor web, base de datos y SSH fueron verificados.  
- Los backups se generan manual y automáticamente.  
- Las particiones adicionales están correctamente configuradas y montadas.  
- Los archivos de entrega están comprimidos según las consignas.  
- Proyecto documentado en este README.md y listo para entrega en GitHub.

---

## 10. Archivos incluidos en el repositorio de GitHub

- `/root.tar.gz`  
- `/etc.tar.gz`  
- `/opt.tar.gz`  
- `/opt_scripts.tar.gz`  
- `/proc.tar.gz`  
- `/proc_part.tar.gz`  
- `/www_dir.tar.gz`  
- `/backup_dir.tar.gz`  
- `/var_part.tar.gzaa`  
- `/var_part.tar.gzab`  
- `README.md`  
- `clave_privada.txt`  
- `clave_publica.pub`

---

Universidad de Palermo – Computación Aplicada (2025)  
Grupo: Bonomi, Feruglio, Frutos, Valle, Sánchez
