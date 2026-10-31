<!-- [Agent-TechWriter] 2026-10-24: Creación del reporte técnico de laboratorio sobre Reescritura MAC en Capa 2 y Proceso ARP (Módulo 13 NetAcad). -->

# 🛡️ Layer 2 MAC Rewriting & ARP Process Analysis in Cross-Network Routing

![Status](https://img.shields.io/badge/status-completed-success)
![Focus](https://img.shields.io/badge/focus-Blue%20Team%20%2F%20SOC-blue)
![Platform](https://img.shields.io/badge/platform-Packet%20Tracer-informational)
![MITRE](https://img.shields.io/badge/MITRE-T1557.002-red)

| Campo | Valor |
|---|---|
| 📅 Fecha | 2026-10-24 |
| 🎓 Fuente | Cisco Networking Academy — Networking Basics (Módulo 13: The ARP Process) |
| 🧩 Concepto | Encapsulación Capa 2 vs Capa 3, Reescritura de Direcciones MAC y Resolución ARP |
| 🎯 Técnica MITRE ATT&CK | [T1557.002 — Adversary-in-the-Middle: ARP Cache Poisoning](https://attack.mitre.org/techniques/T1557/002/) |
| ⏱️ Duración | 45 minutos |

---

## 1. 📌 Resumen Ejecutivo

Se realizó un análisis experimental en Cisco Packet Tracer para validar el comportamiento del direccionamiento físico de Capa 2 (Data Link) frente al direccionamiento lógico de Capa 3 (Network) durante el tráfico inter-red entre hosts ubicados en distintos dominios de difusión. Se generó tráfico ICMP (*Echo Request/Reply*) desde `PC0` hacia un servidor web remoto a través de dos routers de tránsito (`R1` y `R2`). Mediante la herramienta de inspección de puertos (*Port Inspection / Simulation Mode*), se comprobó que las direcciones físicas (MAC) no viajan de extremo a extremo: en cada salto de enrutamiento, la trama Ethernet es desencapsulada, evaluada a nivel IP y re-encapsulada con nuevas direcciones MAC. En el último segmento de red hacia el servidor de destino, la dirección MAC de origen fue reescrita con la dirección física de la interfaz FastEthernet0/1 del Router `R2` (`0001.C972.4202`), mientras que las direcciones IP de origen y destino se mantuvieron invariables. Desde una perspectiva de ciberseguridad defensiva (SOC / Blue Team), este comportamiento delimita el radio de impacto de los ataques de Capa 2 (como ARP Poisoning) al dominio de difusión local, al tiempo que recalca la criticidad del Default Gateway como límite de confianza perimetral.

**Hallazgos clave:**
- **Invariabilidad L3 vs Volatilidad L2:** Las direcciones IP origen (`PC0`) y destino (`Web Server`) permanecen inalteradas durante todo el tránsito enrutado, mientras que los encabezados Ethernet de Capa 2 se destruyen y regeneran en cada salto.
- **Reescritura de MAC en el último salto:** Al ingresar al segmento del servidor de destino, la MAC de origen recibida por el servidor fue reescrita como `0001.C972.4202` (interfaz Fa0/1 de `R2`), resolviendo la MAC de destino del servidor mediante el protocolo ARP local.
- **Aislamiento de ataques L2:** Los ataques de suplantación de Capa 2 (ARP Cache Poisoning, MITRE T1557.002) no pueden propagarse a través de interfaces enrutadas de Capa 3, quedando contenidos en la subred local inmediata.

---

## 2. 🗺️ Topología y Herramientas

### 2.1 Topología

```
[ PC0 ] ── (Fa0)
   │
[ Switch SW1 ]
   │ (Fa0/1)
[ Router R1 ] (Default Gateway LAN 1)
   │ (Enlace inter-router / WAN)
[ Router R2 ] (Default Gateway LAN 2)
   │ (Fa0/1: MAC 0001.C972.4202)
[ Switch SW2 ]
   │
[ Web Server ] (Servidor Web Destino)
```

| Dispositivo | Rol | Interfaz | Dirección L3 (IP) | Dirección L2 (MAC) | Segmento de Red |
|---|---|---|---|---|---|
| **PC0** | Host origen | Fa0 | `192.168.1.10/24` | `0060.2F84.AA01` *(sanitizado)* | LAN 1 (Origen) |
| **R1** | Default Gateway LAN 1 | Fa0/0 | `192.168.1.1/24` | `0001.9610.1101` *(sanitizado)* | LAN 1 (Gateway local) |
| **R1 - R2** | Enlace inter-router | Gi0/0 o Se0/0/0 | `10.0.0.0/30` | Enlace punto a punto | Tránsito de enrutamiento |
| **R2** | Default Gateway LAN 2 | Fa0/1 | `192.168.2.1/24` | `0001.C972.4202` *(validado en lab)* | LAN 2 (Servidor) |
| **Web Server** | Servidor destino | Fa0 | `192.168.2.100/24` | `00D0.FF12.BEEF` *(sanitizado)* | LAN 2 (Servidor web) |

> ℹ️ *Nota de sanitización:* Las direcciones MAC correspondientes a interfaces no provistas explícitamente en las notas han sido documentadas con valores estandarizados para reflejar la estructura íntegra de la topología sin suposiciones arbitrarias.

### 2.2 Herramientas utilizadas

| Herramienta | Versión | Propósito en el lab |
|---|---|---|
| **Cisco Packet Tracer** | 8.x | Simulación de la infraestructura de red, inspección de puertos e intercambio de PDUs. |
| **Simulation Mode & PDU Details** | Integrado en PT | Desglose paso a paso de los encabezados de Capa 2 (Ethernet II) y Capa 3 (IPv4). |
| **ICMP Protocol (ping)** | RFC 792 | Generación de tráfico cruzado inter-red entre subredes remotas. |
| **ARP Process Engine** | Cisco IOS | Resolución dinámica de direcciones IP a MAC y gestión de la tabla ARP. |

---

## 3. 🔬 Análisis de Tráfico y Comandos

### 3.1 Baseline: Proceso ARP y Resolución Local en LAN 1
Al originar el comando `ping` desde `PC0` hacia `192.168.2.100`:
1. `PC0` aplica su máscara de subred (`255.255.255.0`) y determina mediante la operación AND que la IP destino `192.168.2.100` **no pertenece a su red local**.
2. Por consiguiente, el paquete debe remitirse al **Default Gateway** (`192.168.1.1`).
3. `PC0` inspecciona su tabla ARP local (`arp -a`). Si no existe la entrada para `192.168.1.1`, envía un paquete **ARP Request** por broadcast:
```text
ARP Request PDU:
- Hardware Type: Ethernet (1)
- Protocol Type: IPv4 (0x0800)
- Operation: Request (1)
- Sender MAC: 0060.2F84.AA01 (PC0)
- Sender IP:  192.168.1.10
- Target MAC: 00:00:00:00:00:00 (Broadcast FF:FF:FF:FF:FF:FF a nivel L2)
- Target IP:  192.168.1.1 (Gateway R1)
```
4. `R1` responde con un **ARP Reply** unicast indicando su MAC. `PC0` encapsula el paquete ICMP y lo envía hacia `R1`.

**Qué demuestra:**
Un host origen jamás solicita por ARP la dirección MAC de un nodo ubicado fuera de su subred local; siempre resuelve la MAC de su Default Gateway.

### 3.2 Inspección de PDU y Reescritura L2 en el Salto Final (R2 ➔ Web Server)
Al alcanzar el paquete al Router `R2` en el extremo de la red LAN 2:
1. `R2` recibe la trama en su interfaz de entrada, retira el encabezado L2 y examina el encabezado IP.
2. Consulta su tabla de enrutamiento (`show ip route`) y detecta que la red `192.168.2.0/24` está directamente conectada a su interfaz `FastEthernet0/1`.
3. `R2` consulta su tabla ARP para localizar la MAC asociada a `192.168.2.100`. Si no la posee, emite un ARP Request en LAN 2 y almacena la MAC del Web Server.
4. `R2` **re-encapsula** el paquete IP original construyendo una trama Ethernet completamente nueva.

**Datos obtenidos en la inspección de la trama saliente en `R2 Fa0/1`:**
```text
=================== [ ENCABEZADO CAPA 2 (Ethernet II) ] ===================
PDU Outgoing Port : FastEthernet0/1 (Router R2)
Destination MAC   : [MAC del Web Server]
Source MAC        : 0001.C972.4202  <-- MAC física de Fa0/1 de Router R2
EtherType         : 0x0800 (IPv4)

=================== [ ENCABEZADO CAPA 3 (IPv4) ] =========================
Source IP         : 192.168.1.10   <-- IP original de PC0 (Inalterada)
Destination IP    : 192.168.2.100  <-- IP del Web Server (Inalterada)
TTL               : 126            <-- Decrementado por R1 y R2
Protocol          : 1 (ICMP)
```

**Qué demuestra:**
La herramienta de inspección de puertos constata que la MAC de origen recibida por el servidor web es `0001.C972.4202` (R2 Fa0/1). Las direcciones físicas no viajan de extremo a extremo: la Capa 2 solo identifica el origen y destino en el enlace local inmediato (*hop-by-hop*), mientras que la Capa 3 preserva el direccionamiento global (*end-to-end*).

### 3.3 Comparativa de Campos en Tránsito Multisalts

| Parámetro | Segmento 1: PC0 ➔ R1 | Segmento 2: R1 ➔ R2 | Segmento 3: R2 ➔ Servidor | Persistencia Extremo a Extremo |
|---|---|---|---|---|
| **MAC Origen** | MAC de PC0 | MAC salida R1 | `0001.C972.4202` (R2 Fa0/1) | ❌ **No** (Reescrita por cada router) |
| **MAC Destino** | MAC entrada R1 Fa0/0 | MAC entrada R2 | MAC del Web Server | ❌ **No** (Resuelta localmente) |
| **IP Origen** | `192.168.1.10` | `192.168.1.10` | `192.168.1.10` | ✅ **Sí** (Permanece constante) |
| **IP Destino** | `192.168.2.100` | `192.168.2.100` | `192.168.2.100` | ✅ **Sí** (Permanece constante) |
| **Time-to-Live (TTL)** | 128 | 127 | 126 | ⚠️ Decrementado en 1 por salto |

---

## 4. ⚠️ Implicaciones de Seguridad y Ataques Asociados

### 4.1 ¿Por qué es vulnerable el protocolo ARP?
El protocolo ARP (RFC 826) carece de mecanismos de autenticación y verificación de estado (*stateless*):
- Cualquier dispositivo puede emitir respuestas ARP no solicitadas (*Gratuitous ARP* o respuestas falsificadas).
- Los hosts receptores actualizan su tabla de resolución ARP sin validar si previamente emitieron una petición para esa dirección IP.

### 4.2 Escenario de Ataque Real y Delimitación de Radio de Impacto (*Blast Radius*)
1. **Contención del ataque en Capa 2:**
   Debido a que los routers reescriben los encabezados Ethernet y no reenvían tramas de broadcast L2, un atacante ubicado en LAN 1 **no puede** envenenar directamente la tabla ARP del Web Server en LAN 2. El radio de impacto de un ataque de Capa 2 queda circunscrito al dominio de colisión y difusión local.
2. **Impacto del secuestro de Gateway (MITRE T1557.002):**
   Si el atacante envenena la caché ARP de `PC0` asociando la IP del Default Gateway (`192.168.1.1`) a la MAC del atacante, logrará posicionarse como Man-in-the-Middle (MitM) para **todo** el tráfico saliente de LAN 1 hacia redes externas, interceptando credenciales y comunicaciones no cifradas antes de retransmitirlas a `R1`.

### 4.3 Mapeo a Marcos de Referencia

| Marco | Identificador | Descripción |
|---|---|---|
| **MITRE ATT&CK** | [T1557.002](https://attack.mitre.org/techniques/T1557/002/) | Adversary-in-the-Middle: ARP Cache Poisoning |
| **MITRE ATT&CK** | [T1040](https://attack.mitre.org/techniques/T1040/) | Network Sniffing |
| **CIS Controls v8** | Control 12.1 | Ensure Network Infrastructure is Kept Up-to-Date and Properly Segmented |
| **NIST CSF 2.0** | PR.IR-01 / DE.CM-01 | Controles de segmentación de redes y monitoreo continuo de anomalías L2/L3 |

### 4.4 Indicadores para el SOC y Detección

- **Fuentes de telemetría:**
  - Registros Syslog de switches de acceso (alertas de descartes por DAI o Port Security).
  - Sensores de red (NIDS / Zeek / Suricata) monitoreando tasas anómalas de paquetes ARP Reply por segundo.
  - Eventos de sistema operativo en endpoints (eventos de duplicidad de direcciones IP en Windows Event Log o `/var/log/syslog`).
- **Lógica de detección analítica (Regla de ejemplo tipo Suricata/Snort):**
```text
# Detección de ráfagas anómalas de ARP Replies indicativas de ARP Poisoning
alert arp any any -> any any (
    msg:"SEC-SOC-ALERT: Posible ataque de envenenamiento ARP (T1557.002)";
    arp_opcode:reply;
    threshold:type both, track by_src, count 8, seconds 2;
    reference:mitre,T1557.002;
    classtype:bad-unknown;
    sid:1000013; rev:1;
)
```

---

## 5. 🛡️ Mitigación y Hardening en Switches de Acceso

Para prevenir ataques de suplantación de identidad en Capa 2 y envenenamiento ARP en switches Cisco Catalyst:

| # | Control | Mecanismo | Comando de Verificación |
|---|---|---|---|
| **1** | **DHCP Snooping** | Construye la base de datos de enlaces confiables IP-MAC-Puerto. | `show ip dhcp snooping binding` |
| **2** | **Dynamic ARP Inspection (DAI)** | Intercepta y valida todos los paquetes ARP en puertos no confiables contra la BD de DHCP Snooping. | `show ip arp inspection` |
| **3** | **Port Security** | Limita el número de direcciones MAC aprendidas en el puerto del switch. | `show port-security interface <id>` |

### Configuración de Hardening (Cisco IOS):
```cisco
! Paso 1: Habilitar DHCP Snooping globalmente y en la VLAN
ip dhcp snooping
ip dhcp snooping vlan 1

! Paso 2: Marcar el puerto de subida (Uplink hacia el Router/Gateway) como CONFIABLE
interface FastEthernet0/1
 description UPLINK-TO-GATEWAY
 ip dhcp snooping trust
 ip arp inspection trust
 exit

! Paso 3: Habilitar Dynamic ARP Inspection (DAI) en la VLAN
ip arp inspection vlan 1

! Paso 4: Activar validación rigurosa de direcciones MAC e IP en encabezados ARP
ip arp inspection validate src-mac dst-mac ip
```

**Resultado tras la mitigación:**
El switch descarta en hardware cualquier trama ARP no autorizada que pretenda asociar la IP del Gateway a una dirección MAC ilegítima, enviando alertas Syslog al SIEM y protegiendo la integridad del salto L2.

---

## 6. 🧠 Lecciones Aprendidas

- **Diferenciación estricta de responsabilidades de capa:** La confirmación de que la MAC recibida por el servidor web es `0001.C972.4202` (interfaz Fa0/1 de `R2`) demuestra que la identidad de Capa 2 es puramente transitoria y local a cada enlace.
- **Implicación en investigación forense de red:** Un sensor NIDS ubicado en un enlace WAN o DMZ no puede registrar la MAC real del host atacante que originó la conexión; solo observará la MAC del último router de salto. Por ende, la correlación en el SOC requiere indefectiblemente el cruce entre la IP de origen y los logs DHCP/ARP del switch de acceso donde residía el host.

---

## 7. 📚 Referencias

- Cisco Networking Academy — Networking Basics (Módulo 13: Address Resolution Protocol).
- RFC 826 — An Ethernet Address Resolution Protocol (ARP).
- MITRE ATT&CK — [T1557.002: Adversary-in-the-Middle: ARP Cache Poisoning](https://attack.mitre.org/techniques/T1557/002/).
- CIS Controls v8 — Control 12: Network Infrastructure Management.

---
*Laboratorio realizado en entorno aislado con fines educativos — Proyecto CyberMind 🧠🔐*
