🧠 🔐 RED TEAM | BLACK BOX WEB SECURITY AUDIT 2026
📌 ABOUT THE PROJECT

Auditoría de seguridad tipo Black Box Pentesting realizada sobre una infraestructura web corporativa basada en tecnologías IIS y ASP.NET legacy.

Este proyecto tiene como objetivo analizar la superficie de ataque expuesta públicamente, identificando riesgos de seguridad, exposición de servicios, fallos de diseño arquitectónico y debilidades en la configuración del sistema.

El enfoque utilizado sigue metodologías de Ethical Hacking con orientación Red Team, simulando un escenario real de evaluación de seguridad desde la perspectiva de un atacante controlado.

🎯 OBJECTIVE

Evaluar la postura de seguridad de una infraestructura web realista mediante la identificación de:

Exposición de servicios públicos
Uso de tecnologías legacy no soportadas
Riesgos de diseño arquitectónico
Información sensible expuesta
Debilidades en controles de acceso y autenticación
🧩 SYSTEM ARCHITECTURE (HIGH LEVEL)
Internet
   ↓
Frontend (Nginx)
   ↓
Backend (IIS 7.5 - ASP.NET Legacy)
   ↓
Web Applications (.aspx)
   ↓
Possible Internal Network (10.10.x.x)
   ↓
Windows / Active Directory Environment (indicios)
⚙️ METHODOLOGY
Black Box Testing Approach
OSINT Reconnaissance
Web Application Fingerprinting
Attack Surface Mapping
Subdomain & Service Enumeration
Logical Architecture Analysis
Risk Classification (CVSS-based estimation)
Kill Chain Modeling (Red Team perspective)
🧪 TOOLS & REFERENCES
Browser-based HTTP inspection
OSINT techniques (reconocimiento público)
Web headers & response analysis
OWASP Top 10 framework
Ethical Hacking methodologies
Threat modeling fundamentals
Security architecture analysis concepts
⚠️ KEY FINDINGS
🔴 Legacy infrastructure detected (IIS 7.5 / ASP.NET 2.0)
🔴 Possible internal IP exposure in HTTP responses
🟠 Administrative subdomains exposed to Internet
🟠 Fragmented authentication / RBAC structure
🟡 Information disclosure in frontend content
🛡️ RESULTS
Complete attack surface mapping
Identification of critical, high, and medium risk issues
Conceptual Red Team Kill Chain modeling
Security posture evaluation of exposed systems
Non-intrusive remediation recommendations
📌 CONCLUSION

This project demonstrates practical capabilities in:

Offensive Cybersecurity (Red Team mindset)
Web infrastructure security analysis
Structured technical reporting
Ethical Hacking applied to real-world scenarios
Risk identification and security architecture evaluation

The analysis highlights the importance of modernizing legacy systems and improving security hardening practices in enterprise environments.

⚠️ DISCLAIMER

This project is developed strictly for educational and ethical security research purposes.
No unauthorized access, exploitation, or disruption of systems was performed.
