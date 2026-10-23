# 🧪 Labs — Blue Team / SOC

Laboratorios prácticos con enfoque defensivo, derivados de los cursos de **Cisco Networking Academy**. Cada lab documenta: resumen ejecutivo, topología, análisis de tráfico, implicaciones de seguridad (mapeadas a MITRE ATT&CK) y mitigación.

> 🔒 Todos los ejercicios se realizan en entornos aislados (Cisco Packet Tracer / máquinas virtuales) con fines educativos.

## 📚 Índice

| Estado | Lab | Concepto | MITRE ATT&CK |
|---|---|---|---|
| 🟡 En curso | Rogue DHCP Server — Detección y DHCP Snooping | DHCPv4 / DORA | T1557.003 |
| ⚪ Planificado | ARP Cache Poisoning (Spoofing) — Detección y Dynamic ARP Inspection (DAI) | ARP / L2 Security | T1557.002 |
| ⚪ Planificado | Segmentación IPv4 + DHCP + análisis del encabezado IPv4 para aislar tráfico | Subnetting / IPv4 header / DHCP | — |

## 📁 Estructura

```
Labs/
└── YYYY-MM-DD_<slug>/
    ├── README.md      # Technical report
    └── assets/        # Topology & Wireshark screenshots
```
