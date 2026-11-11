<!-- [Agent-Architect] 2026-10-22: Actualización integral del README central alineado con la estrategia Blue Team / SOC y hoja de ruta de empleabilidad remota. -->
<!-- [Agent-TechWriter] 2026-10-31: Añadidas 2 insignias Cisco (Networking Basics, Introduction to Cybersecurity) y carpeta Labs/Technical_Reports en la estructura. -->
<!-- [Agent-Architect] 2026-11-09: Integración de Linux & Kali CLI Mastery, SOC Analyst Blueprint (4 piezas del candado) y escuadrón de agentes de estudio. -->

# CyberMind 🧠🔐

![Status](https://img.shields.io/badge/status-active-success?style=flat-square)
![Focus](https://img.shields.io/badge/focus-Blue%20Team%20%2F%20SOC-blue?style=flat-square)
![Methodology](https://img.shields.io/badge/methodology-80%2F20%20Hands--on-orange?style=flat-square)
![Frameworks](https://img.shields.io/badge/frameworks-MITRE%20ATT%26CK%20%7C%20CIS%20v8%20%7C%20NIST-red?style=flat-square)
![Target](https://img.shields.io/badge/target-Remote%20Security%20Analyst-informational?style=flat-square)

[![Cisco Networking Basics](https://img.shields.io/badge/Cisco-Networking%20Basics-049fd9?style=flat-square&logo=cisco&logoColor=white)](https://www.credly.com/badges/06b55848-1e82-4d4b-b177-d993599777fe/public_url)
![Cisco Introduction to Cybersecurity](https://img.shields.io/badge/Cisco-Introduction%20to%20Cybersecurity-049fd9?style=flat-square&logo=cisco&logoColor=white)
<!-- [PENDIENTE] Añadir enlace Credly de la insignia 'Introduction to Cybersecurity' (no disponible en inbox/raw_notes.md). -->

Repositorio central y portafolio técnico de mi camino hacia roles de **Junior SOC Analyst / Blue Team / Cloud Security**. Este espacio documenta aprendizaje práctico acelerado mediante *Proof of Work* (evidencias reproducibles, análisis de tráfico, detección de amenazas y mitigación verificada).

---

## 📑 Tabla de Contenidos
- [🎯 Visión y Hoja de Ruta](#-visión-y-hoja-de-ruta)
- [🛡️ Blueprint de Operaciones SOC & Triada Defensiva](#️-blueprint-de-operaciones-soc--triada-defensiva)
- [🧪 Laboratorios Prácticos (Blue Team / SOC)](#-laboratorios-prácticos-blue-team--soc)
- [🧱 Fases de Desarrollo Técnico](#-fases-de-desarrollo-técnico)
- [🛠️ Metodología y Herramientas](#️-metodología-y-herramientas)
- [📂 Estructura del Repositorio](#-estructura-del-repositorio)

---

## 🎯 Visión y Hoja de Ruta

El proyecto aplica la **Regla 80/20** y la filosofía de **«Paso firme, vista larga y mala leche»**:
* **80% Práctica Técnica:** Creación de topologías, captura e inspección de paquetes con Wireshark, administración avanzada en Kali Linux CLI y ejercicios prácticos de detección y respuesta en SIEM.
* **20% Documentación de Alto Impacto:** Transformación de notas de laboratorio en reportes técnicos estandarizados y mini-informes ejecutivos de incidente (**Identificación**, **Acción** y **Riesgo**).

> 📄 Consulta la estrategia detallada y los objetivos de carrera en:  
> **[Hoja de Ruta Estratégica (Roadmap Remoto)](./docs/career_roadmap_remote_soc.md)**

---

## 🛡️ Blueprint de Operaciones SOC & Triada Defensiva

El proyecto articula la detección y el triaje (*triage*) de incidentes mediante una conexión directa entre tres pilares:
1. **Linux CLI & Logs Locales:** Auditoría de eventos en `/var/log/auth.log`, `syslog` y `journalctl`.
2. **El Radar Central (SIEM):** Correlación de eventos y reglas de detección en **Wazuh** y **Splunk**.
3. **El Microscopio Forense:** Inspección profunda de paquetes con **Wireshark** y `tcpdump` (`.pcap`).

> 📄 Consulta el diseño de la triada y el modelo de las 4 piezas de empleabilidad en:  
> **[SOC Analyst Blueprint: Linux CLI, Wireshark & SIEM](./docs/soc_analyst_blueprint.md)**

---

## 🧪 Laboratorios Prácticos (Blue Team / SOC)

En la sección **[Labs/](./Labs)** se documentan laboratorios prácticos con enfoque defensivo derivados de cursos oficiales de redes y seguridad (**Cisco Networking Academy**, **TryHackMe**):

* **Topología y configuración baseline.**
* **Simulación controlada de vulnerabilidad / ataque** en entorno aislado.
* **Análisis de tráfico con Wireshark / logs de sistema**, mapeados a técnicas de **MITRE ATT&CK** y controles de **CIS Controls v8**.
* **Mitigación técnica** (Cisco IOS / Linux Hardening) y verificación con comandos de auditoría.

👉 **[Ver índice completo de laboratorios en Labs/README.md](./Labs/README.md)**

---

## 🧱 Fases de Desarrollo Técnico

| Fase | Enfoque Principal | Estado |
|---|---|---|
| **[Phase 1: Foundations](./Phase_1_Foundations)** | Redes (Cisco CCNA ITN / Networking Basics), Wireshark, [Linux & Kali CLI Mastery](./Phase_1_Foundations/Linux_Kali_CLI) (6 niveles), Bash y automatización con Python. | 🟡 En progreso |
| **Phase 2: SOC Operations & SIEM** | Plataformas prácticas (TryHackMe SOC Level 1), análisis de logs (Sysmon/Syslog), despliegue de SIEM (Wazuh/Splunk) y CompTIA Security+. | ⚪ Planificado |
| **Phase 3: Defensive Engineering & Cloud** | Google Cloud Platform Security (IAM, VPC, Audit Logs), automatización de detección y preparación para entrevistas remotas. | ⚪ Planificado |

---

## 🛠️ Metodología y Herramientas

* **Estándar de Idioma:** Código, variables, configuración de equipos y mensajes de commit de Git se redactan en **Inglés** siguiendo las mejores prácticas de la industria global. La documentación y explicaciones analíticas se redactan en **Español** para garantizar claridad conceptual.
* **Terminal-First Workflow:** La práctica real de TI y ciberseguridad se ejecuta principalmente desde la línea de comandos. En equipos con Windows, el entorno recomendado es **WSL2 + Ubuntu/Kali** para aprender comandos reales de Linux y preparar un perfil técnico profesional.
* **Stack de Herramientas:**
  * **Simulación y Redes:** Cisco Packet Tracer, GNS3, Wireshark, `tcpdump`, `nmap`.
  * **Sistemas y Scripting:** Linux (Debian, Kali Linux CLI), Windows PowerShell, Python 3, Bash.
  * **Defensa y Logs:** Sysmon, Zeek, Suricata, Wazuh, Splunk Free.
  * **Marcos de Referencia:** MITRE ATT&CK, CIS Controls v8, NIST CSF 2.0.

---

## 📂 Estructura del Repositorio

```text
CyberMind/
├── docs/                   # Documentación estratégica, roadmaps y blueprints
│   ├── career_roadmap_remote_soc.md  # Hoja de ruta hacia rol remoto SOC 2027
│   ├── soc_analyst_blueprint.md      # Framework de 4 piezas, triada defensiva y tracker
│   ├── cv_technical_profile.md       # Perfil ATS optimizado
│   ├── social_launch_kit.md          # Kit de publicaciones profesionales
│   └── family_business_strategy.md
├── Labs/                   # Reportes técnicos de laboratorios defensivos
│   ├── README.md           # Índice de laboratorios
│   ├── Technical_Reports/  # Reportes de cierre de Networking Basics (L3, L4, L7)
│   ├── Networking_Devices_and_Initial_Configuration/  # Estructura de 19 labs de Cisco
│   └── YYYY-MM-DD_<slug>/  # Reporte técnico + capturas (assets)
├── Phase_1_Foundations/    # Código de fundamentos (Linux, Python, Redes)
│   ├── Linux_Kali_CLI/     # Programa de 6 niveles de maestría en terminal Kali
│   │   ├── README.md       # Hoja de ruta y reglas del instructor
│   │   └── 01_survival_and_identity.md # Labs 1.1 y 1.2
│   ├── Python_Basics/      # Scripts de automatización en Python
│   └── README.md
└── README.md               # Este archivo
```

---
*Road to Remote Junior Cybersecurity Analyst — 2027*
