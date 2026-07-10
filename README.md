```markdown
# 🧠 RED TEAM | AUDITORÍA WEB BLACK BOX – IIS/ASP.NET LEGACY (2026)

![Audit Type](https://img.shields.io/badge/audit-black%20box-red)
![Methodology](https://img.shields.io/badge/methodology-Red%20Team%20%7C%20Kill%20Chain-orange)
![Risk](https://img.shields.io/badge/risk%20level-CRÍTICO-critical)

---

## 📌 SOBRE EL PROYECTO

Auditoría de seguridad ofensiva tipo **Black Box** realizada sobre una infraestructura web corporativa expuesta en Internet.  
El objetivo fue emular un ataque real con enfoque **Red Team**, identificando la superficie de ataque, vulnerabilidades estructurales y exposiciones que permitieran un compromiso completo sin conocimiento previo del sistema.

Se evaluaron tecnologías **IIS 7.5**, **ASP.NET 2.0**, frontales **Nginx** y aplicaciones heredadas con más de 15 años de antigüedad, revelando fallos críticos en diseño, configuración y exposición de información sensible.

Este repositorio contiene el **informe técnico ejecutivo** derivado del ejercicio, diseñado como referencia educativa para profesionales de seguridad ofensiva y arquitectos de sistemas.

---

## 🎯 OBJETIVO

- Simular un ataque controlado sobre la infraestructura pública `victima-corp.com`.
- Mapear completamente la superficie de exposición.
- Identificar riesgos de criticidad alta y crítica en tecnologías legadas.
- Modelar la cadena de ataque (*Kill Chain*) hasta la red interna.
- Proponer contramedidas técnicas no triviales y basadas en hardening real.

---

## 🧩 ARQUITECTURA DEL SISTEMA (ALTO NIVEL)

```
                             Internet
                                │
                                ▼
                     ┌────────────────────┐
                     │  Nginx 1.14.0      │  ← reverse proxy público
                     │  (203.0.113.25)    │
                     └─────────┬──────────┘
                               │ tráfico HTTP interno (NO cifrado)
                               ▼
                     ┌────────────────────────┐
                     │  IIS 7.5 + ASP.NET 2.0 │  ← backend legacy
                     │  (10.10.1.25)          │
                     └───────────┬────────────┘
                                 │
                   ┌─────────────┼─────────────┐
                   ▼             ▼             ▼
            portal/login   admin/       Telerik UI
            (.aspx)        (.aspx)      (.axd handlers)
                   │
                   ▼
         Red interna Windows / AD (10.10.x.x)
         (indicadores de DC en 10.10.1.5)
```

El diseño expone una falta de segmentación real: el backend se encuentra en la misma red lógica que los controladores de dominio, y las fugas de información permiten conocer direccionamiento interno desde el exterior.

---

## ⚙️ METODOLOGÍA

El ejercicio siguió una metodología propia combinando **OSINT**, **enumeración activa**, **análisis de configuración** y **modelado de amenazas**, siempre desde una perspectiva Red Team.

| Fase | Descripción |
|------|-------------|
| 1. **Reconocimiento OSINT** | Búsqueda de subdominios, certificados, información pública. |
| 2. **Fingerprinting** | Detección de tecnologías, servidores, versiones de frameworks. |
| 3. **Mapeo de superficie** | Escaneo de puertos, fuzzing de directorios, enumeración de endpoints sensibles. |
| 4. **Análisis de fugas** | Inspección de cabeceras HTTP, respuestas del servidor, errores y trazas. |
| 5. **Modelado de Kill Chain** | Construcción de una cadena de ataque viable desde reconocimiento hasta pivoting. |
| 6. **Evaluación de riesgos** | Puntuación CVSS estimada y matriz de impacto. |
| 7. **Recomendaciones avanzadas** | Mitigaciones técnicas y arquitectónicas sin soluciones triviales. |

---

## 🧪 HERRAMIENTAS Y TÉCNICAS EMPLEADAS

| Herramienta/Técnica | Propósito |
|---------------------|-----------|
| `crt.sh` + `jq` | Enumeración de subdominios por transparencia de certificados. |
| `whatweb` | Fingerprinting pasivo de tecnologías web. |
| `curl -I` y análisis manual | Detección de fugas en cabeceras (IP interna, versiones). |
| `gobuster` con wordlist IIS/ASP.NET | Fuzzing de directorios y archivos sensibles (.axd, .config, .bak). |
| Análisis de handlers Telerik | Identificación de componentes con CVE públicos. |
| Modelo Cyber Kill Chain de Lockheed Martin adaptado | Representación del ataque en 7 fases. |
| CVSS v3.1 | Clasificación de severidad de los hallazgos. |

**No se utilizaron herramientas automatizadas de explotación**; todo el análisis se basó en el tráfico legítimo y en la interpretación de las respuestas del servidor.

---

## ⚠️ HALLAZGOS PRINCIPALES

### 🔴 Crítico – Infraestructura completamente obsoleta (EOL)
- **Componente:** IIS 7.5 / ASP.NET 2.0.50727  
- **Evidencia:** Cabecera `X-AspNet-Version: 2.0.50727` en todas las respuestas del backend.  
- **Impacto:** El framework dejó de recibir soporte en 2011. Existen múltiples RCE públicos (CVE-2010-3332, MS10-070) aplicables si se encuentra un vector de inyección.  
- **CVSS estimado:** 9.8 (AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)

### 🔴 Crítico – Exposición de handler Telerik vulnerable (CVE-2017-9248)
- **Endpoint:** `/Telerik.Web.UI.WebResource.axd`  
- **Evidencia:** Respuesta 200 a peticiones directas, permite carga de archivos sin autenticación por deserialización insegura.  
- **Impacto:** Ejecución remota de código (RCE) como `IIS APPPOOL\DefaultAppPool` y pivoting inmediato.  
- **CVSS estimado:** 9.8 (exploit público disponible)

### 🟠 Alto – Fuga de dirección IP interna en cabecera `Location`
- **Evidencia:** Redirección 302 devuelve `Location: http://10.10.1.25/portal/login.aspx`  
- **Impacto:** Revela la topología de la red interna, facilitando el reconocimiento post-explotación.  
- **CVSS estimado:** 7.5 (C:I:L, pero alto valor para el atacante)

### 🟠 Alto – `trace.axd` habilitado en producción
- **Endpoint:** `/trace.axd` accesible sin autenticación.  
- **Impacto:** Muestra la traza de ejecución de las páginas, incluyendo datos de sesión, variables de servidor y posibles tokens.  
- **CVSS estimado:** 7.5

### 🟠 Alto – `elmah.axd` sin restricción de acceso
- **Endpoint:** `/elmah.axd` muestra todos los errores de la aplicación.  
- **Impacto:** Divulga cadenas de conexión, rutas internas de archivos y detalles de excepciones.  
- **CVSS estimado:** 8.6

### 🟡 Medio – Subdominios administrativos expuestos
- **Subdominios:** `admin.victima-corp.com`, `cpanel.victima-corp.com`  
- **Impacto:** Amplían la superficie de ataque. `admin` solicita autenticación básica HTTP, pero permite ataques de fuerza bruta.  
- **CVSS estimado:** 6.5

---

## 🗺️ SUPERFICIE DE ATAQUE COMPLETA (MAPA)

```
203.0.113.25:443 (Nginx)
├── / (Portal Corporativo)
├── /login.aspx → redirige a 10.10.1.25 (IP interna)
├── /Telerik.Web.UI.WebResource.axd (RCE potencial)
├── /trace.axd (depuración pública)
├── /elmah.axd (registro de errores público)
├── /admin (subdominio, HTTP Basic)
├── /cpanel (subdominio, redirige a puerto 2083 interno)
├── /owa (Outlook Web App, potencial blanco)
└── /App_Data/ (acceso denegado, confirma existencia)
```

---

## 💀 KILL CHAIN CONCEPTUAL (RED TEAM)

Se modeló un escenario realista de compromiso total, demostrando la viabilidad de una intrusión sin conocimiento previo.

| Fase | Técnica | Herramienta / Método |
|------|---------|----------------------|
| **1. Reconnaissance** | Descubrimiento de subdominios, fingerpr. | crt.sh, whatweb |
| **2. Weaponization** | Preparación de exploit para Telerik | Script Python (público) para CVE-2017-9248 |
| **3. Delivery** | Subida de `cmd.aspx` mediante handler vulnerable | POST multiparte al handler Telerik |
| **4. Exploitation** | Ejecución de comandos en el backend IIS | `curl -X POST -d "cmd=whoami"` a shell plantada |
| **5. Installation** | Descarga de agente C2 (Covenant) | PowerShell sin disco (`powershell -enc ...`) |
| **6. C2** | Canal HTTPS reverso a servidor atacante | Beacon HTTPS con certificado autofirmado |
| **7. Actions on Objectives** | Movimiento lateral a DC `10.10.1.5` | Pass-the-Hash con Mimikatz, PsExec |

El atacante obtendría control total de la red interna partiendo únicamente de la dirección IP pública del balanceador.

---

## 📊 MATRIZ DE RIESGO

| Hallazgo | Probabilidad | Impacto | Riesgo |
|----------|--------------|---------|--------|
| IIS/ASP.NET legacy (EOL) | Muy alta | Crítico | 🔴 Crítico |
| Telerik UI vulnerable | Alta | Crítico | 🔴 Crítico |
| Fuga de IP interna | Alta | Alto | 🟠 Alto |
| trace.axd habilitado | Media | Alto | 🟠 Alto |
| elmah.axd público | Media | Alto | 🟠 Alto |
| Subdominios administrativos | Media | Medio | 🟡 Medio |

---

## 🛡️ RECOMENDACIONES TÉCNICAS (NO BÁSICAS)

1. **Mitigar fuga de IP interna:**  
   En Nginx, usar `proxy_redirect http://10.10.1.25/ https://victima-corp.com/;` y eliminar headers de ubicación no deseados.  
   Adicionalmente, configurar `more_clear_headers 'Location'` (módulo headers-more) si no se puede modificar la aplicación.

2. **Deshabilitar diagnósticos:**  
   En `web.config`:
   ```xml
   <trace enabled="false" localOnly="true" />
   ```
   Eliminar el handler de ELMAH o protegerlo con autenticación integrada y regla de firewall de aplicación.

3. **Actualizar/eliminar Telerik:**  
   Si no es necesario, eliminar el handler `Telerik.Web.UI.WebResource.axd`.  
   Caso contrario, migrar a la última versión (2023+) y aplicar hardening de serialización.

4. **Migrar plataforma:**  
   Planificar la actualización a **IIS 10 / ASP.NET Core 8** con contenedores o servidores modernos.  
   A corto plazo, habilitar **modo administrado de .NET 4.8** en lugar de 2.0, si la compatibilidad lo permite.

5. **Aislar segmentos de red:**  
   - Colocar el Nginx en una **DMZ** dedicada.  
   - El backend IIS debe residir en una subred separada sin acceso directo a controladores de dominio.  
   - Filtrar tráfico entre segmentos mediante firewall interno (ej. pfSense).

6. **Implementar WAF con reglas personalizadas:**  
   Bloquear accesos a `*.axd` desde el exterior.  
   Configurar ModSecurity con reglas OWASP Core Rule Set y reglas específicas contra CVE de Telerik.

7. **Reforzar la postura de autenticación:**  
   Sustituir HTTP Basic en `admin` por autenticación multifactor y limitar por IP de origen.

8. **Monitoreo y detección:**  
   - Alertar sobre accesos a `trace.axd`, `elmah.axd` o cualquier handler Telerik.  
   - Implementar SIEM con reglas de detección de escaneo de subdominios y patrones de ataque a estos vectores.

---

## 📌 CONCLUSIÓN

Este proyecto demuestra que una infraestructura aparentemente simple (un frontal Nginx y un backend IIS) puede ser completamente vulnerable debido a la **obsolescencia tecnológica** y la **falta de hardening en configuraciones por defecto**.  
La combinación de fugas de información, componentes con CVE públicos y una red interna plana permite que un atacante externo, sin privilegios, pueda comprometer todo el dominio en un tiempo mínimo.

**Competencias prácticas evidenciadas:**
- Enumeración avanzada sin depender de escáneres automáticos.
- Modelado de amenazas realista (Kill Chain).
- Interpretación de artefactos legacy y su explotabilidad.
- Generación de informes técnicos accionables para equipos de defensa.

---

## ⚠️ DISCLAIMER

Este repositorio y su contenido se publican **exclusivamente con fines educativos y de investigación en seguridad**.  
Todas las pruebas fueron realizadas en un entorno controlado y simulado. No se ejecutó ninguna intrusión sobre sistemas reales sin autorización.  
El uso indebido de la información aquí presentada es responsabilidad exclusiva del lector.

---

**Autor:** CyberZenithAI  
**Tipo de auditoría:** Red Team / Black Box  
**Versión del informe:** 1.0 – Julio 2026
```
