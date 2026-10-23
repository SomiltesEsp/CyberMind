<!-- [Agent-Architect] 2026-10-22: Actualización integral del README central alineado con la estrategia Blue Team / SOC y hoja de ruta de empleabilidad remota. -->

# CyberMind 🧠🔐

![Status](https://img.shields.io/badge/status-active-success?style=flat-square)
![Focus](https://img.shields.io/badge/focus-Blue%20Team%20%2F%20SOC-blue?style=flat-square)
![Methodology](https://img.shields.io/badge/methodology-80%2F20%20Hands--on-orange?style=flat-square)
![Frameworks](https://img.shields.io/badge/frameworks-MITRE%20ATT%26CK%20%7C%20CIS%20v8%20%7C%20NIST-red?style=flat-square)
![Target](https://img.shields.io/badge/target-Remote%20Security%20Analyst-informational?style=flat-square)

Repositorio central y portafolio técnico de mi camino hacia roles de **Junior SOC Analyst / Blue Team / Cloud Security**. Este espacio documenta aprendizaje práctico acelerado mediante *Proof of Work* (evidencias reproducibles, análisis de tráfico, detección de amenazas y mitigación verificada).

---

## 📑 Tabla de Contenidos
- [🎯 Visión y Hoja de Ruta](#-visión-y-hoja-de-ruta)
- [🧪 Laboratorios Prácticos (Blue Team / SOC)](#-laboratorios-prácticos-blue-team--soc)
- [🧱 Fases de Desarrollo Técnico](#-fases-de-desarrollo-técnico)
- [🛠️ Metodología y Herramientas](#️-metodología-y-herramientas)
- [📂 Estructura del Repositorio](#-estructura-del-repositorio)

---

## 🎯 Visión y Hoja de Ruta

El proyecto aplica la **Regla 80/20**:
* **80% Práctica Técnica:** Creación de topologías, captura e inspección de paquetes con Wireshark, administración en Linux/Windows y ejercicios prácticos de detección y respuesta.
* **20% Documentación de Alto Impacto:** Transformación de notas de laboratorio en reportes técnicos estandarizados y publicaciones especializadas para la comunidad infosec.

> 📄 Consulta la estrategia detallada y los objetivos de carrera en:  
> **[Hoja de Ruta Estratégica (Roadmap Remoto)](./docs/career_roadmap_remote_soc.md)**

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
| **[Phase 1: Foundations](./Phase_1_Foundations)** | Redes (Cisco CCNA ITN / Networking Basics), Wireshark, Linux CLI, Bash y automatización con Python. | 🟡 En progreso |
| **Phase 2: SOC Operations & SIEM** | Plataformas prácticas (TryHackMe SOC Level 1), análisis de logs (Sysmon/Syslog), despliegue de SIEM (Wazuh/Splunk) y CompTIA Security+. | ⚪ Planificado |
| **Phase 3: Defensive Engineering & Cloud** | Google Cloud Platform Security (IAM, VPC, Audit Logs), automatización de detección y preparación para entrevistas remotas. | ⚪ Planificado |

---

## 🛠️ Metodología y Herramientas

* **Estándar de Idioma:** Código, variables, configuración de equipos y mensajes de commit de Git se redactan en **Inglés** siguiendo las mejores prácticas de la industria global. La documentación y explicaciones analíticas se redactan en **Español** para garantizar claridad conceptual.
* **Stack de Herramientas:**
  * **Simulación y Redes:** Cisco Packet Tracer, GNS3, Wireshark, `tcpdump`, `nmap`.
  * **Sistemas y Scripting:** Linux (Ubuntu/Debian, Kali), Windows PowerShell, Python 3, Bash.
  * **Defensa y Logs:** Sysmon, Zeek, Suricata, Wazuh, Splunk Free.
  * **Marcos de Referencia:** MITRE ATT&CK, CIS Controls v8, NIST CSF 2.0.

---

## 📂 Estructura del Repositorio

```text
CyberMind/
├── docs/                   # Documentación estratégica, roadmaps y guías
│   ├── career_roadmap_remote_soc.md
│   └── family_business_strategy.md
├── Labs/                   # Reportes técnicos de laboratorios defensivos
│   ├── README.md           # Índice de laboratorios
│   └── YYYY-MM-DD_<slug>/  # Reporte técnico + capturas (assets)
├── Phase_1_Foundations/    # Código de fundamentos (Python, Linux, Networking)
│   ├── Python_Basics/
│   └── README.md
└── README.md               # Este archivo
```

---
*Road to Remote Junior Cybersecurity Analyst — 2027*
