<!-- [Agent-TechWriter] 2026-10-31: Creación del reporte L4 (Módulo 15 — TCP and UDP) a partir de inbox/raw_notes.md. Sin outputs inventados; huecos marcados como [PENDIENTE]. -->

# 🛡️ Layer 4 — Auditoría de Conexiones TCP/UDP con `netstat` y Puertos Efímeros

![Status](https://img.shields.io/badge/status-completed-success)
![Focus](https://img.shields.io/badge/focus-Blue%20Team%20%2F%20SOC-blue)
![Platform](https://img.shields.io/badge/platform-Cisco%20Networking%20Academy-informational)
![MITRE](https://img.shields.io/badge/MITRE-T1049-red)

| Campo | Valor |
|---|---|
| 📅 Fecha | 2026-10-31 |
| 🎓 Fuente | Cisco Networking Academy — Networking Basics, Módulo 15 (*TCP and UDP*) |
| 🧩 Concepto | Multiplexación L4, *socket pairs*, puertos efímeros, estado `ESTABLISHED` |
| 🎯 Técnica MITRE ATT&CK | [T1049 — System Network Connections Discovery](https://attack.mitre.org/techniques/T1049/) *(contexto defensivo)* |
| 📝 Evaluación del módulo | Quiz: **91 %** |
| ⏱️ Duración | [PENDIENTE: no registrada en las notas] |

---

## 1. 📌 Resumen Ejecutivo

Se estudió cómo la capa de transporte permite que un mismo host mantenga múltiples conversaciones simultáneas mediante **multiplexación** de puertos, y cómo identificar cada conversación como un **par de sockets** (IP + puerto en cada extremo). Se practicó la auditoría de conexiones activas con `netstat`, prestando atención a las conexiones en estado `ESTABLISHED` y a los **puertos efímeros** que el cliente asigna a cada sesión.

Para un analista SOC, este conocimiento es la base para responder a la pregunta *"¿con quién está hablando este equipo y por qué puerto?"*, primer paso al triar un host sospechoso.

**Hallazgos clave:**
- Cada conexión TCP/UDP se identifica de forma única por su *socket pair* (IP origen:puerto origen ↔ IP destino:puerto destino).
- El cliente usa un **puerto efímero** (dinámico) como origen, mientras el servicio escucha en un **puerto bien conocido** (p. ej. 443 para HTTPS).
- `netstat` permite enumerar conexiones activas y detectar sesiones `ESTABLISHED` que no deberían existir.

## 2. 🗺️ Topología y Herramientas

### 2.1 Topología
> ⚠️ [PENDIENTE: las notas no describen topología ni dispositivos para esta práctica. Añadir diagrama en `assets/` si aplica.]

### 2.2 Herramientas utilizadas
| Herramienta | Versión | Propósito en el lab |
|---|---|---|
| `netstat` | [PENDIENTE] | Enumerar conexiones activas, puertos locales/remotos y estados |
| Cisco Networking Academy (Módulo 15) | — | Material teórico y quiz de evaluación |

## 3. 🔬 Análisis de Tráfico y Comandos

### 3.1 Conceptos base: multiplexación y *socket pair*

| Elemento | Rol | Ejemplo de puertos |
|---|---|---|
| Puerto bien conocido (0–1023) | Servicio que *escucha* | 80 (HTTP), 443 (HTTPS), 53 (DNS) |
| Puerto efímero (dinámico) | Origen asignado por el cliente por cada sesión | Rango IANA recomendado: 49152–65535 |
| *Socket pair* | Identificador único de una conversación | `IP_cliente:puerto_efímero ↔ IP_servidor:443` |

> ℹ️ El rango efímero exacto depende del sistema operativo (por ejemplo, Linux usa 32768–60999 por defecto). En Packet Tracer se observó el puerto efímero **5001** (ver [reporte Capa 7](./layer7_https_pdu_inspection_dns.md)).

### 3.2 Auditoría con `netstat`

```cmd
netstat -an
netstat -ano
```

| Opción | Función |
|---|---|
| `-a` | Muestra todas las conexiones y puertos en escucha |
| `-n` | Muestra direcciones y puertos en formato numérico (sin resolver nombres) |
| `-o` | Añade el PID del proceso propietario (Windows) |

**Cómo se lee la salida:** columnas `Proto`, `Local Address`, `Foreign Address` y `State`. Una sesión en `ESTABLISHED` indica un *three-way handshake* completado y una conexión activa.

> ⚠️ [PENDIENTE: pegar aquí el output real de `netstat` capturado durante la práctica y señalar las líneas `ESTABLISHED` analizadas. Las notas del 2026-10-31 no incluyen el output literal.]

**Qué demuestra:** que es posible inventariar en tiempo real las conversaciones L4 de un host y correlacionar cada puerto efímero con su servicio remoto.

## 4. ⚠️ Implicaciones de Seguridad y Ataques Asociados

### 4.1 ¿Por qué importa?
`netstat` es una herramienta de **doble uso**: el defensor la utiliza para auditar; el adversario, para descubrir servicios y conexiones tras comprometer un host. Una conexión `ESTABLISHED` hacia un destino o puerto inesperado es un indicador clásico de *beaconing* o de *reverse shell*.

### 4.2 Escenario de ataque real
Un equipo de usuario comprometido mantiene una sesión TCP saliente hacia un servidor externo en un puerto inusual. Al ser saliente, suele atravesar el firewall sin bloqueo. **Impacto:** confidencialidad (exfiltración) e integridad (control remoto). *(Escenario conceptual; no fue reproducido en este lab.)*

### 4.3 Mapeo a marcos de referencia
| Marco | Referencia | Descripción |
|---|---|---|
| MITRE ATT&CK | [T1049](https://attack.mitre.org/techniques/T1049/) | Descubrimiento de conexiones de red del sistema |
| MITRE ATT&CK | [T1571](https://attack.mitre.org/techniques/T1571/) | Uso de puertos no estándar (referencia contextual) |
| CIS Controls v8 | 4.5 | Firewall en dispositivos de usuario final |
| CIS Controls v8 | 13.1 | Centralizar alertas de eventos de seguridad |

### 4.4 Indicadores para el SOC
- **IoC / IoA:** conexiones `ESTABLISHED` hacia puertos no estándar; procesos inesperados con sockets abiertos; puertos en `LISTENING` no inventariados.
- **Fuente de logs:** telemetría de conexiones de red del endpoint (p. ej. Sysmon Event ID 3), logs de firewall.
- **Lógica de detección (pseudo-regla):**
```text
IF connection.state == "ESTABLISHED"
   AND connection.direction == "outbound"
   AND dest_port NOT IN approved_ports
THEN alert("Unusual outbound connection") severity=medium
```

## 5. 🛡️ Mitigación

> 🔎 Las medidas siguientes son **recomendaciones derivadas del análisis**; las notas no indican que se hayan aplicado en este lab.

| # | Control | Configuración | Verificación |
|---|---|---|---|
| 1 | Línea base de puertos en escucha | Documentar servicios autorizados por host | `netstat -an` y comparar contra la línea base |
| 2 | Firewall de host con *egress filtering* | Permitir solo puertos de salida necesarios | Reintentar conexión bloqueada y confirmar ausencia de `ESTABLISHED` |
| 3 | Atribución de procesos | Identificar PID propietario | `netstat -ano` + administrador de tareas |

## 6. 🧠 Lecciones Aprendidas
- Dominar la diferencia entre puerto de servicio y puerto efímero acelera la lectura de capturas y logs.
- Resultado del módulo: **91 %** en el quiz de TCP y UDP.
- > ⚠️ [PENDIENTE: registrar errores o dudas específicas de esta práctica; las notas no los incluyen.]

## 7. 📚 Referencias
- Cisco Networking Academy — Networking Basics, Módulo 15: *TCP and UDP*
- [MITRE ATT&CK T1049](https://attack.mitre.org/techniques/T1049/)
- [IANA Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/)

---
*Laboratorio realizado en entorno aislado con fines educativos — Proyecto CyberMind 🧠🔐*
