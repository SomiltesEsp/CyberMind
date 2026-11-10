<!-- [Agent-TechWriter] 2026-11-09: Documentación técnica del Laboratorio 1.1 y 1.2 de Linux & Kali CLI Mastery. Mapeado a técnicas de MITRE ATT&CK T1082 y T1033. -->

# Lab 1.1 & 1.2: Reconocimiento Inicial, Identidad y Anatomía del FHS en Kali Linux 🐧🔍

> **Nivel:** 1 — Supervivencia en CLI y Estructura del Sistema  
> **Frameworks:** MITRE ATT&CK [T1082 (System Information Discovery)](https://attack.mitre.org/techniques/T1082/), [T1033 (System Owner/User Discovery)](https://attack.mitre.org/techniques/T1033/), [T1083 (File and Directory Discovery)](https://attack.mitre.org/techniques/T1083/)  
> **Entorno:** Kali Linux (Host nativo o Máquina Virtual)

---

## 🎯 Objetivos Técnicos

1. Reconocer la identidad de ejecución, identificadores de seguridad (UID, GID, grupos secundarios) y la versión del kernel en menos de 10 segundos desde la terminal.
2. Identificar interfaces de red locales y direccionamiento IP asignado sin recurrir a interfaces gráficas.
3. Comprender la función e implicación en seguridad de los directorios clave del estándar **FHS (*Filesystem Hierarchy Standard*)**: `/etc`, `/var`, `/proc`, `/tmp`, `/bin` y `/dev`.
4. Generar evidencias auditables en disco usando el comando `tee` para el portafolio técnico de GitHub.

---

## 🧭 Lección 1.1: Identidad, Entorno y Privilegios

### Concepto
En auditorías de seguridad, respuesta a incidentes o administración de servidores, lo primero que hace un operador al obtener una shell es orientarse: saber con qué usuario opera, si pertenece al grupo administrativo (`sudo`), qué arquitectura de kernel soporta el host y en qué ruta absoluta está situado. Operar a ciegas como `root` vulnera el principio de menor privilegio.

### Demostración y Desglose de Comandos
```bash
{ echo "=== IDENTIDAD ==="; whoami; id; echo "=== RUTA ==="; pwd; echo "=== KERNEL ==="; uname -srm; echo "=== RED ==="; ip -br a; }
```
- `whoami`: Imprime el nombre del usuario efectivo en la sesión actual.
- `id`: Desglosa UID (*User ID*), GID (*Group ID*) y membresía a grupos de seguridad (ej. `sudo`, `adm`).
- `pwd`: (*Print Working Directory*) Muestra la ruta absoluta del directorio de trabajo actual.
- `uname`: Muestra información del sistema operativo:
  - `-s`: Nombre del kernel (`Linux`).
  - `-r`: Versión de lanzamiento del kernel (ej. `6.x.x-amd64`).
  - `-m`: Arquitectura del hardware (`x86_64` o `aarch64`).
- `ip -br a`: Muestra las interfaces de red (`eth0`, `wlan0`, `lo`) y sus direcciones IPv4/IPv6 de forma tabular y compacta (`-br` = *brief*).

**Salida Típica Esperada en Kali:**
```text
=== IDENTIDAD ===
kali
uid=1000(kali) gid=1000(kali) groups=1000(kali),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)
=== RUTA ===
/home/kali
=== KERNEL ===
Linux 6.11.2-amd64 x86_64
=== RED ===
lo               UNKNOWN        127.0.0.1/8 ::1/128 
eth0             UP             192.168.1.105/24 fe80::a00:27ff:fe8d:e6c1/64
```

---

## 📂 Lección 1.2: Anatomía del FHS y Directorios Críticos

### Concepto
Linux organiza sus archivos bajo una jerarquía unificada (**FHS**). Para un analista defensivo o administrador, saber exactamente dónde reside la configuración del sistema, dónde se almacenan los registros de auditoría y dónde los procesos exponen su estado en memoria es la diferencia entre detectar un intruso a tiempo o pasar por alto una intrusión.

### Mapa de Directorios Críticos para Seguridad:

| Directorio | Tipo / Propósito | Relevancia en Seguridad / SOC |
|---|---|---|
| `/etc` | Archivos de configuración estáticos del sistema. | Contiene `/etc/passwd` (cuentas locales), `/etc/shadow` (hashes de contraseñas, solo legible por root), `/etc/sudoers` y configuraciones de servicios (`/etc/ssh/sshd_config`). |
| `/var/log` | Registros del sistema y servicios. | El corazón del análisis forense local. Contiene `auth.log` (intentos de acceso SSH y comandos `sudo`), `syslog`, `nginx/`, `apache2/`. |
| `/proc` | Sistema de archivos virtual montado en RAM por el kernel. | Expone información de procesos activos en tiempo real (`/proc/<PID>/`), hardware (`/proc/cpuinfo`) y red (`/proc/net/`). |
| `/tmp` | Archivos temporales de lectura y escritura universal. | Espacio habitual donde atacantes descargan scripts o cargas maliciosas debido a sus permisos de escritura abierta (`1777` con Sticky Bit). |
| `/dev` | Nodos de dispositivos especiales. | Contiene `/dev/null` (el "agujero negro" para descartar salidas de error) y accesos a discos crudos (`/dev/sda`). |

---

## 🛠️ Procedimiento de Laboratorio y Reto Práctico

Ejecuta los siguientes pasos ordenados en tu terminal de Kali Linux:

### Paso 1: Creación del entorno de prácticas
```bash
mkdir -p ~/lab-soc/nivel1 && cd ~/lab-soc/nivel1
```

### Paso 2: Generación del reporte de estado inicial
Ejecuta el bloque combinado usando `tee` para ver la información en pantalla y guardarla simultáneamente en disco:
```bash
{
  echo "=========================================="
  echo "    REPORTE DE ESTADO INICIAL KALI LINUX   "
  echo "=========================================="
  echo -e "\n[+] Fecha y Hora:"; date -u
  echo -e "\n[+] Usuario e Identidad de Seguridad:"; id
  echo -e "\n[+] Directorio Actual de Trabajo:"; pwd
  echo -e "\n[+] Kernel y Arquitectura:"; uname -srm
  echo -e "\n[+] Interfaces de Red y Direcciones IP:"; ip -br a
  echo -e "\n[+] Espacio en Disco (Partición Raíz):"; df -h /
} | tee estado_inicial.txt
```

### Paso 3: Inspección de permisos de archivo
Verifica con qué permisos y qué propietario creó Linux el archivo de registro:
```bash
ls -la estado_inicial.txt
```

---

## ✅ Criterio de Verificación de Laboratorio

Para dar por completado este laboratorio y habilitar el siguiente reto (Nivel 1.3: Manipulación segura y búsquedas de archivos ocultos):
1. Confirma que el archivo `~/lab-soc/nivel1/estado_inicial.txt` existe y tiene tamaño mayor a cero.
2. Identifica tu número de `uid` y si tu usuario tiene membresía en el grupo `sudo` (GID 27 en Debian/Kali).
3. Interpreta la cadena de permisos resultante de `ls -la estado_inicial.txt` (ej. `-rw-r--r--`).

---

*CyberMind Security Operations — Proof of Work Documented.*
