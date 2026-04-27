🧠 🔐 RED TEAM | BLACK BOX WEB SECURITY AUDIT 2026
📌 ABOUT THE PROJECT

Auditoría de seguridad tipo Black Box Pentesting aplicada a infraestructura web corporativa basada en IIS y ASP.NET legacy.

Este proyecto analiza la superficie de ataque expuesta públicamente, identificando riesgos de seguridad, exposición de servicios y debilidades estructurales mediante metodologías de Ethical Hacking y enfoque Red Team.

🎯 OBJECTIVE

Evaluar la postura de seguridad de una infraestructura web realista, detectando:

Exposición de servicios públicos
Uso de tecnologías legacy
Posibles riesgos de diseño arquitectónico
Información sensible expuesta
Debilidades en control de acceso
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
Web Fingerprinting
Attack Surface Mapping
Subdomain & Service Enumeration
Logical Architecture Analysis
Risk Classification (CVSS-based)
Kill Chain Modeling (Red Team Perspective)
🧪 TOOLS & REFERENCES
Browser-based HTTP inspection
OSINT techniques (Google Dorking concepts)
Web headers analysis
OWASP Top 10 framework
Ethical Hacking methodologies
Threat modeling concepts
⚠️ KEY FINDINGS
🔴 Legacy infrastructure detected (IIS 7.5 / ASP.NET 2.0)
🔴 Possible internal IP disclosure in responses
🟠 Exposed administrative subdomains
🟠 Fragmented authentication / RBAC structure
🟡 Information disclosure in frontend content
🛡️ RESULTS
Complete attack surface mapping
Identification of critical and high-risk issues
Conceptual Kill Chain modeling
Security posture evaluation
Remediation recommendations (non-intrusive)
📌 CONCLUSION

This project demonstrates practical knowledge in offensive cybersecurity (Red Team), structured technical reporting, and real-world web infrastructure analysis using ethical hacking principles.

⚠️ DISCLAIMER

This project is developed for educational and ethical security research purposes only. No real systems were harmed or exploited.
