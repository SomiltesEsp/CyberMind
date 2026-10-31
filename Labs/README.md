<!-- [Agent-TechWriter] 2026-10-24: Incorporación del reporte de Reescritura MAC L2 y Proceso ARP al índice de labs. -->
<!-- [Agent-TechWriter] 2026-10-31: Añadidos 3 reportes de cierre de Networking Basics (L3/L4/L7) y enlace al scaffold del curso Networking Devices and Initial Configuration. -->

# 🧪 Labs — Blue Team / SOC

Laboratorios prácticos con enfoque defensivo, derivados de los cursos de **Cisco Networking Academy**. Cada lab documenta: resumen ejecutivo, topología, análisis de tráfico, implicaciones de seguridad (mapeadas a MITRE ATT&CK) y mitigación.

> 🔒 Todos los ejercicios se realizan en entornos aislados (Cisco Packet Tracer / máquinas virtuales) con fines educativos.

## 📚 Índice

| Estado | Lab | Concepto | MITRE ATT&CK |
|---|---|---|---|
| 🟢 Completado | [Layer 2 MAC Rewriting & ARP Process Analysis](./2026-10-24_layer2-mac-rewriting-arp/) | Encapsulación L2 vs L3 / Reescritura MAC / ARP | [T1557.002](https://attack.mitre.org/techniques/T1557/002/) |
| 🟢 Completado | [Layer 4 — netstat & Ephemeral Ports](./Technical_Reports/layer4_netstat_ephemeral_ports.md) | TCP/UDP / Socket pairs / Puertos efímeros | [T1049](https://attack.mitre.org/techniques/T1049/) |
| 🟢 Completado | [Layer 7 — HTTPS PDU Inspection & DNS](./Technical_Reports/layer7_https_pdu_inspection_dns.md) | HTTPS 443 / DNS / IPv4 público vs. privado | [T1071.001](https://attack.mitre.org/techniques/T1071/001/) · [T1071.004](https://attack.mitre.org/techniques/T1071/004/) |
| 🟢 Completado | [Layer 3 — show ip interface brief & tracert (R1)](./Technical_Reports/layer3_r1_show_ip_interface_brief_tracert.md) | Estado de interfaces / Default gateway / Traza de ruta | [T1016](https://attack.mitre.org/techniques/T1016/) |
| 🗂️ Estructura lista | [Networking Devices and Initial Configuration (19 labs)](./Networking_Devices_and_Initial_Configuration/) | Próximo curso Cisco | — |
| 🟡 En curso | Rogue DHCP Server — Detección y DHCP Snooping | DHCPv4 / DORA | T1557.003 |
| ⚪ Planificado | ARP Cache Poisoning (Spoofing) — Detección y Dynamic ARP Inspection (DAI) | ARP / L2 Security | T1557.002 |
| ⚪ Planificado | Segmentación IPv4 + DHCP + análisis del encabezado IPv4 para aislar tráfico | Subnetting / IPv4 header / DHCP | — |

## 📁 Estructura

```
Labs/
├── Technical_Reports/                              # Networking Basics closure reports (L3, L4, L7)
├── Networking_Devices_and_Initial_Configuration/   # 19 lab scaffolds (next course)
└── YYYY-MM-DD_<slug>/
    ├── README.md      # Technical report
    └── assets/        # Topology & Wireshark screenshots
```
