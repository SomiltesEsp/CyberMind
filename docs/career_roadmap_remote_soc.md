<!-- [Agent-Architect] 2026-10-22: Creación de la hoja de ruta estratégica y metodología de aceleración profesional hacia roles remotos en Ciberseguridad Defensiva (SOC/Blue Team). -->
<!-- [Agent-Architect] 2026-11-09: Actualización mayor integrando el framework de las 4 piezas del candado de empleabilidad (RINKU), la triada defensiva (Linux CLI + Wireshark + SIEM) y el escuadrón de agentes de estudio interactivo. -->

# 🗺️ Hoja de Ruta Estratégica: De Cero a Ingeniero de Ciberseguridad Remoto

> **Visión:** Construir un perfil técnico altamente empleable, respaldado por evidencia pública e irrebatible (*Proof of Work*), para acceder a posiciones remotas internacionales en dólares (Junior SOC Analyst / Blue Team / Cloud Security Analyst) hacia 2027.

---

## 🧭 1. Principios Fundamentales

### 1.1 El Candado de las 4 Piezas y la Mentalidad Operativa
Inspirado en la filosofía de **Pablo López (RINKU)** (*«Paso firme, vista larga y mala leche»*):
* **Paso firme:** Sumar práctica real y un comando nuevo todos los días sin interrumpir la consistencia.
* **Vista larga:** Mantener el foco fijo en el objetivo de Analista SOC sin reiniciar el camino cada semana saltando entre modas.
* **Mala leche:** Transformar bloqueos y frustraciones técnicas en disciplina y rigor para investigar a fondo.

El acceso al primer empleo formal se concibe como un **candado de 4 números**: si faltan piezas, el candado no abre:
1. **Fundamentos Reales:** Redes (TCP/IP, DNS, ARP), Sistemas Operativos (Linux FHS, procesos, permisos) y análisis de paquetes con Wireshark sin atajos ni memorización hueca.
2. **Operar Entornos Ajenos:** Salir del "laboratorio con final feliz"; resolver problemas en infraestructuras y máquinas que no configuraste tú.
3. **Protocolo de Desatasque (Troubleshooting Propio):** Leer el error completo → aislar partes pequeñas → buscar literal en documentación oficial → límite de tiempo de consulta → documentar la solución.
4. **Explicación al Negocio:** Cerrar cada incidente y laboratorio con un mini-informe ejecutivo de 1 página (**Identificación**, **Acción** y **Riesgo/Mitigación**).

> 📄 Para la especificación completa de este framework y la plantilla de reporte, consulta:  
> **[SOC Analyst Blueprint: Linux CLI, Wireshark & SIEM](./soc_analyst_blueprint.md)**

### 1.2 La Regla 80 / 20
Para evitar el *"Tutorial Hell"* y maximizar el retorno de inversión sobre el tiempo:
* **80% Práctica Técnica Intensiva:** Laboratorios reales, captura e inspección de paquetes (Wireshark), administración de sistemas (Linux/Bash, Windows Server), simulación de redes e infraestructura (Cisco Packet Tracer, GNS3), y plataformas prácticas (TryHackMe, Blue Team Labs Online).
* **20% Documentación y Marca Personal:** Automatización asistida por agentes para transformar notas crudas en reportes técnicos de alta calidad para GitHub y publicaciones estratégicas de divulgación técnica en LinkedIn, X, Threads e Instagram.

### 1.3 Honestidad Técnica y Credibilidad
* El perfil se posiciona como **Junior / Especialista en Formación Continua**.
* Sin exageraciones: se demuestra rigor mediante la calidad del análisis de causa raíz, la comprensión profunda de protocolos y la aplicación de marcos estándar de la industria (**MITRE ATT&CK**, **CIS Controls v8**, **NIST CSF 2.0**).
* Los errores y la resolución de problemas (*troubleshooting*) se documentan como evidencia de pensamiento crítico y resiliencia.

---

## 🎯 2. Objetivos de Rol y Mercado Objetivo

* **Roles Primarios:**
  * Junior SOC Analyst (Tier 1)
  * Junior Defensive / Blue Team Analyst
  * Junior Security Operations Center Analyst
  * IT Security Support Specialist
* **Modalidad:** 100% Remoto (Latinoamérica, EE.UU., Europa / Global).
* **Rango Salarial Objetivo Inicial:** $2,000 – $4,000+ USD mensuales.

---

## 🏗️ 3. Fases de Desarrollo Técnico

```mermaid
flowchart LR
    A[Fase 1: Fundamentos, Linux & Redes] --> B[Fase 2: Operaciones SOC, SIEM & Detección]
    B --> C[Fase 3: Cloud Security & Caza Activa]
```

### Fase 1: Fundamentos de Sistemas, Redes y Tráfico (En Curso)
* **Redes (Cisco Networking Academy - CCNA ITN / Networking Basics):**
  * Modelo OSI y TCP/IP a fondo.
  * Análisis de cabeceras de paquetes (IPv4, IPv6, TCP, UDP, ICMP).
  * Segmentación de redes, subnetting (VLSM) y direccionamiento dinámico (DHCP/DORA).
  * Protocolos de soporte y resolución (ARP, DNS, NAT, VLANs, STP).
* **Análisis de Tráfico:** Dominio de Wireshark y `tcpdump` para detección de anomalías y tráfico malicioso no autenticado.
* **Sistemas & Scripting (Linux & Kali CLI):**
  * Dominio del FHS (`/etc`, `/var`, `/proc`, `/tmp`), gestión de permisos (`chmod`, `chown`), procesos, servicios systemd y principio de menor privilegio (`sudo`).
  * Análisis de registros de autenticación (`/var/log/auth.log`, `journalctl`).
  * Python y Bash para automatización de tareas de seguridad e interacción con sockets/APIs.
* **Entregables:** Mínimo 15 laboratorios documentados en el directorio [Labs/](../Labs/) y completar los 6 niveles de [Phase_1_Foundations/Linux_Kali_CLI/](../Phase_1_Foundations/Linux_Kali_CLI/).

### Fase 2: Operaciones Defensivas, SIEM y Respuesta a Incidentes
* **Plataformas Prácticas:** TryHackMe (Rutas *Pre-Security*, *Cyber Defense* y *SOC Level 1*), LetsDefend, CyberDefenders.
* **Gestión de Eventos e Información de Seguridad (SIEM):**
  * Despliegue y configuración de laboratorios con **Wazuh** o **Splunk Free**.
  * Ingesta de logs de Windows Event Viewer (Sysmon) y Linux Syslog/Auditd.
  * Creación y afinamiento de reglas de detección basadas en MITRE ATT&CK.
* **Certificaciones Clave:**
  * Cisco Junior Cybersecurity Analyst Career Path.
  * Preparación y obtención de **CompTIA Security+** (SY0-701) o **BTL1 (Blue Team Level 1)**.

### Fase 3: Seguridad en la Nube y Aceleración de Empleabilidad
* **Cloud Security (GCP):**
  * Fundamentos de IAM, VPC Service Controls, Cloud Audit Logs y Cloud Armor en Google Cloud Platform.
  * Alineación hacia la certificación *Google Cloud Associate Cloud Engineer / Professional Security Engineer*.
* **Estrategia de Búsqueda Activa:**
  * Currículum vitae optimizado para sistemas ATS (Applicant Tracking Systems) con métricas y enlaces directos a código.
  * Red de contactos en LinkedIn con reclutadores técnicos, SOC Leads y profesionales de ciberseguridad.
  * Práctica de entrevistas técnicas estructuradas bajo el método STAR (Situación, Tarea, Acción, Resultado).

---

## 🤖 4. Arquitectura de Agentes de Estudio y Flujo Técnico

El aprendizaje activo y la construcción del portafolio se estructuran en dos capas complementarias de agentes:

```mermaid
flowchart TD
    subgraph Estudio_Laboratorio ["Capa 1: Escuadrón de Práctica Diaria"]
        KM["kali-master (Instructor CLI)"]
        NA["net-architect (Mentor Redes / Wireshark)"]
        SD["soc-defender (Analista SIEM & Logs)"]
        DS["doc-scribe (Curador de Portafolio)"]
    end
    subgraph Pipeline_Editorial ["Capa 2: Pipeline de Automatización Editorial"]
        OR["Orchestrator"]
        LA["Lab Architect"]
        TW["Tech Writer"]
        SE["Social Engine"]
        CH["Career Hunter"]
    end
    Estudio_Laboratorio --> Pipeline_Editorial
    Pipeline_Editorial --> Repo[(GitHub CyberMind & Redes)]
```

### Capa 1: Escuadrón de Estudio Activo (Laboratorio)
* **`kali-master`:** Maestro de terminal, guía los retos prácticos en Kali Linux con validación de outputs.
* **`net-architect`:** Desglosa el tráfico en Wireshark y `tcpdump` para cada comando ejecutado.
* **`soc-defender`:** Simula el triaje defensivo y correlación de alertas en SIEM (Wazuh).
* **`doc-scribe`:** Convierte la sesión de laboratorio en un reporte técnico y mini-informe ejecutivo.

### Capa 2: Pipeline Editorial y Aceleración de Carrera
* **`Orchestrator`:** Triage de notas crudas, control de calidad y orquestación del flujo.
* **`Lab Architect`:** Transforma módulos de estudio en mini-labs prácticos con MITRE ATT&CK.
* **`Tech Writer`:** Convierte comandos y outputs en reportes técnicos profesionales para `Labs/`.
* **`Social Engine`:** Sintetiza los reportes técnicos en publicaciones de alto impacto (LinkedIn, X).
* **`Career Hunter`:** Audita ofertas de trabajo remotas, detecta brechas de habilidades y optimiza CV para ATS.

---

## 📊 5. Indicadores de Éxito (KPIs)

1. **Volumen de Evidencia Pública:** Mínimo 2 a 3 reportes técnicos de laboratorio completos publicados mensualmente en [Labs/](../Labs/).
2. **Consistencia de Publicación:** Presencia técnica semanal continua en LinkedIn y X documentando aprendizajes y mitigaciones.
3. **Métricas de Código:** Repositorio público limpio, estructurado y sin código monolítico ("Zero Spaghetti").
4. **Dominio de Detección:** Capacidad demostrada para explicar cualquier ataque documentado desde la perspectiva del defensor (registro, paquete, alerta, mitigación).
5. **Tracker de Retos:** Avance continuo en el roadmap de 30 retos prácticos de Linux CLI, Wireshark y SIEM.

---

*CyberMind — Road to Remote Junior Cybersecurity Analyst — 2027*
