<!-- [Agent-TechWriter] 2026-10-31: Creación del reporte L7 (Módulos 16-17 — Lab 1 PDU HTTPS y Lab 3 DNS) a partir de inbox/raw_notes.md. Sin outputs inventados; huecos marcados como [PENDIENTE]. -->

# 🛡️ Layer 7 — Inspección de PDU HTTPS (443) y Resolución DNS

![Status](https://img.shields.io/badge/status-completed-success)
![Focus](https://img.shields.io/badge/focus-Blue%20Team%20%2F%20SOC-blue)
![Platform](https://img.shields.io/badge/platform-Cisco%20Packet%20Tracer-informational)
![MITRE](https://img.shields.io/badge/MITRE-T1071.001%20%7C%20T1071.004-red)

| Campo | Valor |
|---|---|
| 📅 Fecha | 2026-10-31 |
| 🎓 Fuente | Cisco Networking Academy — Networking Basics, Módulos 16 y 17 (*Application Layer* y *Testing Utilities*) |
| 🧩 Concepto | HTTPS sobre TCP/443, puertos efímeros, DNS, direccionamiento IPv4 público vs. privado |
| 🎯 Técnica MITRE ATT&CK | [T1071.001 — Web Protocols](https://attack.mitre.org/techniques/T1071/001/) · [T1071.004 — DNS](https://attack.mitre.org/techniques/T1071/004/) |
| 📝 Evaluación | Checkpoint (Módulos 16–17): **75 %** |
| ⏱️ Duración | [PENDIENTE: no registrada en las notas] |

---

## 1. 📌 Resumen Ejecutivo

Se analizó cómo viaja una petición de capa de aplicación: primero mediante la **inspección de una PDU HTTPS** en Packet Tracer, donde se identificó el servicio por el puerto de origen 443 y el puerto de destino efímero 5001; después, configurando manualmente un host con direccionamiento IPv4 y **servidor DNS** para resolver el nombre `www.server2.com`. Además se practicó la distinción entre direcciones IPv4 públicas y rangos privados RFC 1918.

Para un SOC, HTTP(S) y DNS son los dos protocolos más abusados como canal de comando y control, porque casi siempre están permitidos por el firewall. Entender su comportamiento normal es el requisito para detectar el anómalo.

**Hallazgos clave:**
- El servicio HTTPS se identificó en la cabecera TCP del PDU: **Source Port 443 → Destination Port 5001 (efímero)**.
- Un host necesita IP, máscara, *default gateway* y **servidor DNS** correctamente configurados para resolver nombres.
- Los rangos privados RFC 1918 (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) no son enrutables en Internet; el resto de IPv4 válidas se consideran públicas.

## 2. 🗺️ Topología y Herramientas

### 2.1 Topología y direccionamiento (Lab 3)

| Dispositivo | Rol | Interfaz | IP / Máscara | Gateway | DNS |
|---|---|---|---|---|---|
| PC0 | Cliente | NIC | [PENDIENTE: IP de host] / `255.255.255.0` | `192.168.1.254` | `198.51.100.5` |

- Red LAN: `192.168.1.0/24`.
- Nombre a resolver: `www.server2.com`.

> ℹ️ `198.51.100.5` pertenece al rango de documentación TEST-NET-2 (RFC 5737).

### 2.2 Herramientas utilizadas
| Herramienta | Propósito en el lab |
|---|---|
| Cisco Packet Tracer — Simulation Mode | Inspección de *Outbound PDU Details* y cabeceras TCP |
| Configuración IP del host (estática) | Provisionamiento de IP, máscara, gateway y DNS |

## 3. 🔬 Análisis de Tráfico y Comandos

### 3.1 Lab 1 — Inspección de PDU HTTPS

| Campo de cabecera TCP | Valor observado | Interpretación |
|---|---|---|
| Source Port | **443** | Puerto bien conocido de HTTPS: el PDU proviene del servidor |
| Destination Port | **5001** | Puerto efímero asignado por el cliente a la sesión |

**Qué demuestra:** que el servicio de aplicación se puede inferir solo con la cabecera de transporte, sin leer el *payload*. Con HTTPS el contenido va cifrado, pero **los metadatos (IPs, puertos, tamaños, tiempos) siguen siendo visibles** para un analista de red.

> ⚠️ [PENDIENTE: añadir captura de *Outbound PDU Details* en `./assets/` y la IP origen/destino del PDU, ya que las notas no las especifican.]

### 3.2 Lab 3 — Provisionamiento IPv4 y resolución DNS

Configuración estática aplicada en PC0:

| Parámetro | Valor |
|---|---|
| Red | `192.168.1.0/24` |
| Máscara | `255.255.255.0` |
| Default Gateway | `192.168.1.254` |
| Servidor DNS | `198.51.100.5` |
| Nombre objetivo | `www.server2.com` |

Verificación recomendada desde la consola del host:

```cmd
nslookup www.server2.com
ping www.server2.com
```

> ⚠️ [PENDIENTE: registrar el resultado real de la resolución. Las notas indican el objetivo (resolver `www.server2.com`) pero no el output.]

### 3.3 Clasificación de direcciones IPv4

| Tipo | Rangos | Enrutable en Internet |
|---|---|---|
| Privada (RFC 1918) | `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` | No |
| Pública | Cualquier IPv4 válida fuera de los rangos anteriores y de rangos reservados | Sí |

## 4. ⚠️ Implicaciones de Seguridad y Ataques Asociados

### 4.1 ¿Por qué es relevante?
- **HTTPS:** el cifrado protege el contenido, no los metadatos; además, el tráfico TCP/443 saliente rara vez se bloquea, lo que lo hace atractivo para canales C2.
- **DNS:** el cliente confía en el servidor DNS configurado. Si se apunta (o se redirige) a un resolutor malicioso, las respuestas pueden falsificarse.

### 4.2 Escenario de ataque real
Un malware utiliza consultas DNS con subdominios largos y de alta entropía para exfiltrar datos (*DNS tunneling*), o establece *beaconing* periódico sobre TCP/443. **Impacto:** confidencialidad (exfiltración) y control del host. *(Escenario conceptual, no reproducido en este lab.)*

### 4.3 Mapeo a marcos de referencia
| Marco | Referencia | Descripción |
|---|---|---|
| MITRE ATT&CK | [T1071.001](https://attack.mitre.org/techniques/T1071/001/) | C2 sobre protocolos web |
| MITRE ATT&CK | [T1071.004](https://attack.mitre.org/techniques/T1071/004/) | C2 sobre DNS |
| CIS Controls v8 | 9.2 | Usar servicios DNS de confianza |
| CIS Controls v8 | 13.1 | Centralizar alertas de eventos de seguridad |

### 4.4 Indicadores para el SOC
- **IoC / IoA:** consultas DNS con subdominios largos o registros TXT frecuentes; conexiones TCP/443 periódicas hacia un mismo destino; hosts usando un DNS distinto al corporativo.
- **Fuente de logs:** logs del resolutor DNS, logs de proxy/firewall, NetFlow.
- **Lógica de detección (pseudo-regla):**
```text
IF dns.query.length > 50 OR dns.query_rate(host, 5m) > baseline * 3
THEN alert("Possible DNS tunneling") severity=high
```

## 5. 🛡️ Mitigación

| # | Control | Configuración | Verificación |
|---|---|---|---|
| 1 | DNS de confianza fijado en el host | Servidor DNS estático (aplicado en Lab 3: `198.51.100.5`) | `ipconfig /all` |
| 2 | Restringir DNS saliente | Permitir puerto 53 solo hacia resolutores autorizados | Consulta a DNS no autorizado debe fallar |
| 3 | Inspección de metadatos HTTPS | Monitorear SNI/IP/volumen en proxy o NetFlow | Revisión de logs |

> 🔎 Solo el control 1 corresponde a lo realizado en el lab; los controles 2 y 3 son recomendaciones de hardening.

## 6. 🧠 Lecciones Aprendidas
- Los puertos efímeros permiten asociar la respuesta del servidor con la sesión del cliente (cf. [reporte Capa 4](./layer4_netstat_ephemeral_ports.md)).
- Resultado del checkpoint de Módulos 16–17: **75 %**; en el examen final, *Servicios de capa de aplicación en tareas reales* obtuvo **70.6 %**, lo que indica margen de mejora en la configuración de servicios en escenarios reales.
- > ⚠️ [PENDIENTE: registrar errores de configuración específicos si los hubo.]

## 7. 📚 Referencias
- Cisco Networking Academy — Networking Basics, Módulos 16 y 17
- [RFC 1918 — Address Allocation for Private Internets](https://www.rfc-editor.org/rfc/rfc1918)
- [RFC 5737 — IPv4 Address Blocks Reserved for Documentation](https://www.rfc-editor.org/rfc/rfc5737)
- [MITRE ATT&CK T1071](https://attack.mitre.org/techniques/T1071/)

---
*Laboratorio realizado en entorno aislado con fines educativos — Proyecto CyberMind 🧠🔐*
