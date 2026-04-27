🧠 🔐 INFORME TÉCNICO DE AUDITORÍA DE SEGURIDAD
Red Team / Black Box Web Security Assessment 2026
📌 1. INFORMACIÓN GENERAL
Tipo de auditoría: Black Box Pentesting
Enfoque: Red Team / Ethical Hacking
Infraestructura: Web corporativa (IIS / ASP.NET / Servicios de correo)
Fecha: 26 Abril 2026
Perfil del auditor: Junior–Intermedio (21 años) – Estudiante SENATI (Ing. Soporte TI)
🧠 2. RESUMEN EJECUTIVO

Se realizó una auditoría de seguridad sobre una infraestructura web expuesta públicamente, identificando riesgos asociados a tecnologías legacy, exposición de servicios y posibles debilidades en la arquitectura.

🔴 Conclusión general:

La infraestructura presenta un nivel de exposición medio-alto, principalmente por el uso de tecnologías obsoletas y servicios administrativos accesibles desde Internet.

🧱 3. ALCANCE DE LA AUDITORÍA

Sistemas evaluados:

Frontend web corporativo (Nginx)
Backend IIS 7.5 (ASP.NET legacy)
Subdominios de servicios:
webmail.*
mail.*
postfixadmin.*
portal.*
Aplicaciones web (.aspx)
🧭 4. ARQUITECTURA IDENTIFICADA
Internet
   ↓
Nginx (Frontend)
   ↓
IIS 7.5 (Backend ASP.NET)
   ↓
Aplicaciones Web (.aspx)
   ↓
Posible red interna (10.10.x.x)
   ↓
Entorno Windows / Active Directory (indicios)
⚠️ 5. HALLAZGOS DE SEGURIDAD
🔴 HALLAZGO 1: TECNOLOGÍA LEGACY
IIS 7.5 + ASP.NET 2.0
Sin soporte moderno ni parches actuales
Riesgo elevado de vulnerabilidades conocidas

Riesgo: CRÍTICO

🔴 HALLAZGO 2: POSIBLE EXPOSICIÓN INTERNA
Referencias a IP interna detectadas (10.10.x.x)
Posible filtración de arquitectura interna

Riesgo: ALTO

🟠 HALLAZGO 3: SUPERFICIE DE ATAQUE EXPUESTA
Subdominios administrativos accesibles públicamente
Servicios de correo expuestos

Riesgo: ALTO

🟡 HALLAZGO 4: INFORMATION DISCLOSURE
Exposición de correos y rutas internas
Metadatos visibles en frontend

Riesgo: MEDIO

🧠 6. MODELO DE ATAQUE (KILL CHAIN)
Reconocimiento (OSINT)
Enumeración de servicios
Análisis de arquitectura
Identificación de exposición interna
Evaluación de control de acceso
Posible explotación lógica (teórica)
📊 7. MATRIZ DE RIESGO
Categoría	Nivel
Tecnología legacy	🔴 Crítico
Exposición interna	🔴 Alto
Subdominios administrativos	🟠 Alto
Autenticación / RBAC	🟠 Alto
Information Disclosure	🟡 Medio
🛡️ 8. RECOMENDACIONES
🔴 CRÍTICAS
Migrar IIS 7.5 a versión soportada (IIS 10+)
Actualizar ASP.NET legacy a versión moderna
Eliminar referencias a IP internas en frontend
🟠 ALTAS
Proteger subdominios con MFA + VPN
Segmentar red (DMZ real)
Revisar control de acceso por roles (RBAC)
🟡 MEDIAS
Minimizar información expuesta en frontend
Ocultar headers tecnológicos
Reducir fingerprinting del servidor
📌 9. CONCLUSIÓN FINAL

La infraestructura evaluada presenta debilidades principalmente en diseño arquitectónico, exposición de servicios y uso de tecnología obsoleta, lo cual incrementa el riesgo de ataque.

Sin embargo, no se identificaron vulnerabilidades explotables directas dentro del alcance de esta auditoría.

⚠️ DISCLAIMER

Este informe es parte de un ejercicio de ciberseguridad ética (Ethical Hacking / Red Team) con fines educativos y de análisis profesional.
