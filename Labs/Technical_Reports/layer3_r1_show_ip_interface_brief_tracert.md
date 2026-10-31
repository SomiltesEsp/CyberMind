<!-- [Agent-TechWriter] 2026-10-31: Creación del reporte L3 (Módulos 16-17 — Lab 2 show ip interface brief y Lab 4 tracert) a partir de inbox/raw_notes.md. Sin outputs inventados; huecos marcados como [PENDIENTE]. -->

# 🛡️ Layer 3 — Diagnóstico de Conectividad: `show ip interface brief` y `tracert` (R1)

![Status](https://img.shields.io/badge/status-completed-success)
![Focus](https://img.shields.io/badge/focus-Blue%20Team%20%2F%20SOC-blue)
![Platform](https://img.shields.io/badge/platform-Cisco%20Packet%20Tracer-informational)
![MITRE](https://img.shields.io/badge/MITRE-T1016-red)

| Campo | Valor |
|---|---|
| 📅 Fecha | 2026-10-31 |
| 🎓 Fuente | Cisco Networking Academy — Networking Basics, Módulo 17 (*Testing Utilities*) |
| 🧩 Concepto | Estado de interfaces IOS, *default gateway*, traza de ruta ICMP |
| 🎯 Técnica MITRE ATT&CK | [T1016 — System Network Configuration Discovery](https://attack.mitre.org/techniques/T1016/) *(contexto defensivo)* |
| 📝 Evaluación | Checkpoint (Módulos 16–17): **75 %** |
| ⏱️ Duración | [PENDIENTE: no registrada en las notas] |

---

## 1. 📌 Resumen Ejecutivo

Se practicó el diagnóstico de conectividad en Capa 3 con dos herramientas complementarias: la CLI de **Cisco IOS** (`enable` + `show ip interface brief`) en el router *Office* para identificar la interfaz que actúa como *default gateway* de PC0, y **`tracert`** entre dos hosts de redes distintas para localizar dónde se interrumpe el camino.

En el segundo ejercicio, el primer salto respondió en 2 ms y el siguiente agotó el tiempo de espera, lo que permitió acotar la falla al router **R1** (o a su reenvío hacia la red destino) en lugar de a la LAN origen.

**Hallazgos clave:**
- La interfaz `GigabitEthernet0/0` del router *Office* es el *default gateway* de PC0.
- `tracert` PC1 (`192.168.1.1`) → PC2 (`10.1.0.2`): salto 1 = `192.168.1.254` (2 ms), salto 2 = `Request timed out`.
- Un primer salto exitoso descarta fallas en el enlace host–gateway y reduce el alcance del troubleshooting.

## 2. 🗺️ Topología y Herramientas

### 2.1 Topología (Lab 4)

```text
PC1 (192.168.1.1) ──── [192.168.1.254] R1 ──── ... ──── PC2 (10.1.0.2)
```
> ⚠️ [PENDIENTE: las notas no detallan los dispositivos intermedios ni las demás interfaces. Añadir diagrama real en `./assets/topology.png`.]

| Dispositivo | Rol | Interfaz | IP / Máscara |
|---|---|---|---|
| PC1 | Origen del `tracert` | NIC | `192.168.1.1` / [PENDIENTE] |
| R1 | Gateway de la LAN `192.168.1.0/24` | LAN | `192.168.1.254` |
| PC2 | Destino | NIC | `10.1.0.2` / [PENDIENTE] |
| Router *Office* (Lab 2) | Gateway de PC0 | `GigabitEthernet0/0` | [PENDIENTE: IP no registrada] |

### 2.2 Herramientas utilizadas
| Herramienta | Propósito en el lab |
|---|---|
| Cisco IOS CLI | Consultar estado y direccionamiento de interfaces |
| `tracert` (Windows / Packet Tracer) | Trazar saltos L3 mediante ICMP/TTL |

## 3. 🔬 Análisis de Tráfico y Comandos

### 3.1 Lab 2 — Identificación del *default gateway* en el router *Office*

```cisco
Office> enable
Office# show ip interface brief
```

**Cómo se lee:** la columna `IP-Address` de `GigabitEthernet0/0` coincide con el *default gateway* configurado en PC0, y las columnas `Status`/`Protocol` deben mostrar `up/up` para que la interfaz sea funcional.

> ⚠️ [PENDIENTE: pegar el output real (recortado con `...`) y la IP de `GigabitEthernet0/0`; las notas confirman la conclusión pero no el output.]

**Qué demuestra:** que el *default gateway* de un host debe ser la IP de la interfaz del router en su mismo segmento (cf. Módulo 14).

### 3.2 Lab 4 — Diagnóstico de ruta con `tracert`

```cmd
tracert 10.1.0.2
```

| Salto | Dirección | Latencia | Interpretación |
|---|---|---|---|
| 1 | `192.168.1.254` | 2 ms | El gateway (R1) responde: LAN origen y enlace host–gateway OK |
| 2 | — | `Request timed out` | El tráfico no progresa más allá de R1 |

**Qué demuestra:** `tracert` aísla el punto de fallo por TTL incremental. Como el salto 1 responde, el problema no está en PC1 ni en su configuración de gateway, sino en el reenvío desde R1 hacia la red `10.1.0.0`.

> ⚠️ [PENDIENTE: causa raíz confirmada (p. ej. ruta faltante, interfaz *down* o ACL) y acción correctiva aplicada; las notas solo registran el aislamiento de la falla en R1.]

## 4. ⚠️ Implicaciones de Seguridad y Ataques Asociados

### 4.1 ¿Por qué es relevante?
Los mismos comandos que usa el defensor para diagnosticar (`show ip interface brief`, `tracert`) sirven al adversario para **mapear la red interna** tras un acceso inicial. Además, un *timeout* a partir de un salto puede deberse tanto a una falla operativa como a un **filtrado deliberado** (ACL/firewall) o a un enrutamiento manipulado.

### 4.2 Escenario de ataque real
Un atacante con acceso a un host ejecuta `tracert` hacia varias subredes para identificar routers, segmentos y puntos de filtrado. **Impacto:** reconocimiento previo al movimiento lateral. *(Escenario conceptual, no reproducido.)*

### 4.3 Mapeo a marcos de referencia
| Marco | Referencia | Descripción |
|---|---|---|
| MITRE ATT&CK | [T1016](https://attack.mitre.org/techniques/T1016/) | Descubrimiento de configuración de red |
| MITRE ATT&CK | [T1018](https://attack.mitre.org/techniques/T1018/) | Descubrimiento de sistemas remotos |
| CIS Controls v8 | 12.2 | Arquitectura de red segura |
| CIS Controls v8 | 8.2 | Recolectar logs de auditoría |

### 4.4 Indicadores para el SOC
- **IoC / IoA:** ráfagas de ICMP Echo con TTL incremental (1, 2, 3…) desde un mismo host; ICMP *Time Exceeded* en volumen.
- **Fuente de logs:** logs de firewall/router, NetFlow, IDS.
- **Lógica de detección (pseudo-regla):**
```text
IF icmp.echo_request.ttl IN (1..10) from same src within 30s
   AND distinct(dst_ip) > 3
THEN alert("Possible traceroute / network mapping") severity=low
```

## 5. 🛡️ Mitigación y Troubleshooting

> 🔎 Pasos recomendados a partir del diagnóstico; las notas no indican su ejecución.

| # | Acción | Comando | Verificación |
|---|---|---|---|
| 1 | Confirmar estado de interfaces de R1 | `show ip interface brief` | Interfaces en `up/up` |
| 2 | Verificar tabla de enrutamiento | `show ip route` | Existe ruta hacia `10.1.0.0` |
| 3 | Revisar filtrado | `show access-lists` | Sin ACL bloqueando ICMP/tráfico legítimo |
| 4 | Limitar ICMP expuesto hacia redes no confiables | ACL específica | `tracert` desde red externa debe fallar |

**Resultado tras la corrección:** [PENDIENTE: no registrado en las notas].

## 6. 🧠 Lecciones Aprendidas
- Un primer salto exitoso seguido de *timeout* orienta el diagnóstico hacia el router, no hacia el host.
- En el examen final, los dominios más débiles fueron **Creación de una LAN simple (52.6 %)** y **Protocolos, dispositivos y medios de red (60.0 %)**; refuerzan la necesidad de practicar configuración de topologías completas, que es el foco del siguiente curso.
- Resultado del checkpoint de Módulos 16–17: **75 %**.

## 7. 📚 Referencias
- Cisco Networking Academy — Networking Basics, Módulo 17
- [MITRE ATT&CK T1016](https://attack.mitre.org/techniques/T1016/)
- [Cisco IOS — `show ip interface brief`](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html)

---
*Laboratorio realizado en entorno aislado con fines educativos — Proyecto CyberMind 🧠🔐*
