<!-- [Agent-Architect] 2026-11-09: Actualización integral de Phase 1 Foundations para integrar formalmente el track de Linux & Kali CLI Mastery junto a Python y Networking. -->

# Phase 1: Foundations 🧱

Esta es la primera fase del viaje. Aquí construimos los cimientos técnicos necesarios antes de entrar de lleno a operaciones complejas de Blue Team o hacking ético defensivo.

---

## 🎯 Objetivos de la Fase

1. **Sistemas Operativos (Linux & Kali CLI):** Desenvolverse cómodamente y con agilidad en la terminal (CLI), dominar la jerarquía del sistema de archivos (FHS), gestionar permisos y grupos bajo el principio de menor privilegio, monitorear procesos y servicios, y analizar registros del sistema (`/var/log/`).
2. **Redes Básicas & Análisis de Tráfico:** Dominar el Modelo OSI, la suite TCP/IP, direccionamiento y segmentación IPv4, puertos efímeros/bien conocidos y análisis de paquetes con Wireshark y `tcpdump`.
3. **Programación en Python:** Comprender sintaxis básica, estructuras de datos, funciones y creación de scripts de automatización e interacción con sockets/APIs para tareas de ciberseguridad.

---

## 🧭 Módulos de Trabajo en Phase 1

### 1. [Linux & Kali CLI Mastery](./Linux_Kali_CLI) 🐧🛡️
Programa progresivo de 6 niveles guiado por el método *«Explicación Breve -> Ejemplo Real -> Prueba Práctica»*:
* [x] **Setup Inicial:** Configuración del entorno de laboratorio en Kali Linux.
* [ ] **Nivel 1:** Supervivencia en CLI, FHS (`/etc`, `/var`, `/proc`, `/tmp`) y manipulación de archivos. 👉 *[Lab 1.1 & 1.2](./Linux_Kali_CLI/01_survival_and_identity.md)*
* [ ] **Nivel 2:** Permisos octales/simbólicos, propietarios, grupos y elevación legítima con `sudo`.
* [ ] **Nivel 3:** Tuberías (`|`), redirecciones (`>`, `>>`, `2>&1`), filtrado forense (`grep`, `awk`, `cut`, `sed`) y búsqueda (`find`).
* [ ] **Nivel 4:** Procesos, servicios systemd (`systemctl`, `journalctl`) y análisis de logs (`/var/log/auth.log`).
* [ ] **Nivel 5:** Diagnóstico de red y reconocimiento en Kali (`ip`, `ss`, `netstat`, `curl`, `nmap`, `nc`, `tcpdump`).
* [ ] **Nivel 6:** Automatización básica con Bash Scripting para tareas de laboratorio.

### 2. [Python Basics for Security](./Python_Basics) 🐍
Fundamentos de programación y scripting defensivo:
* [x] **01_password_generator.py:** Generador de contraseñas seguras con entropía configurable y manejo de librerías estándar (`secrets`, `string`).
* [ ] **Escáner de Puertos Básico:** Interacción con sockets de red en Python (`socket`).
* [ ] **Analizador de Cabeceras HTTP:** Script de inspección de seguridad en peticiones web (`urllib`/`requests`).

### 3. Redes y Análisis de Tráfico 🌐
Laboratorios prácticos de redes derivados de cursos oficiales de Cisco Networking Academy:
* 👉 **[Consultar reportes técnicos de redes en Labs/Technical_Reports](../Labs/Technical_Reports)**:
  * Capa 3: Diagnóstico con `show ip interface brief` y aislamiento de fallos con `tracert`.
  * Capa 4: Auditoría de conexiones activas, sockets y puertos efímeros con `netstat`.
  * Capa 7: Inspección de PDU HTTPS en TCP/443 y resolución DNS.

---

*Phase 1: Foundations — CyberMind Security Operations*
