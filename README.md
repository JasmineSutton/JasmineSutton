![Matrix Rain banner](https://github.com/JasmineSutton/JasmineSutton/blob/main/GitHub_MatrixRain_Banner_5.gif)



<!--
**JasmineSutton/JasmineSutton** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.
-->

#  Jasmine Sutton | Security Engineering / Governance, Risk, and Compliance

I’m a cybersecurity M.S. student with a background in data engineering and systems integration, now focused on **security engineering, governance, and defensible systems**.

I’m especially interested in environments where security decisions need to be traceable, evidence matters, and systems are expected to hold up under pressure. I’m comfortable working in documentation-heavy environments and treating data handling as a priority security concern.

**What I focus on**
- translating risk into practical technical controls like validation, safer defaults, and audit logging
- integrity and traceability: correctness, auditability, and predictable behavior
- incident-style thinking: scope, evidence, impact, remediation, and lessons learned
- AI-assisted development with a judgment layer: verify, harden, and document before it ships

Right now that looks like:
- teaching secure AI-assisted development as a CodePath Technical Fellow
- building and hardening Python projects with security controls layered into the design
- doing hands-on labs and competition work, and writing things up clearly
- connecting a strong data and systems background to applied security, governance, and defensible system design

---

## Competition & Hands-On Work

### National Centers of Academic Excellence in Cybersecurity (NCAE) Cyber Games - March 2026
Participated in the NCAE Cyber Games, a national collegiate cybersecurity competition designed to simulate real-world defensive operations through hands-on technical and incident-response scenarios.
Completed the technical inject on competition day, including SQL injection execution, malicious payload deployment for file extraction, live attack tracking through Gravwell SIEM, and incident reporting with remediation recommendations. 
This work required technical execution, situational awareness, and clear written documentation under competition conditions.

### West Coast Regional Collegiate Cyber Defense Competition (WRCCDC) / CCDC Invitational - 2025-2026
Participated in CCDC, a hands-on cybersecurity competition focused on defending and operating an existing network infrastructure while responding to live threats and business-driven tasks.
Worked across multiple competition roles, including Blue Team participation, host-school support, inject creation, and Orange Team operations. 
Contributed to system hardening, service validation, incident triage, and live competition support in a scenario-driven environment. 
Also offered a permanent Red Team spot, reflecting growing involvement across both defensive and offensive security work.
Participated in a WRCCDC research study involving Anthropic’s Claude, where the model was used both as a separate Blue Team and as support for student Red Teamers. 
This experience gave me exposure to defensive operations, competition support, offensive-growth opportunities, and AI-assisted security research within the CCDC environment.

### Cybersecurity Labs & Practical Work
Completed hands-on labs in wireless security, mobile hardening, digital forensics, network analysis, vulnerability assessment, and incident response. 
This work has focused on applying security concepts in practical technical environments and strengthening investigative, analytical, and defensive skills.

---

## Writing & Security Artifacts

Documentation is part of security. It preserves intent, supports audits, and makes systems easier to defend.

When I harden a system, I aim to capture the "why" behind controls: assumptions, threat model, and how the control is verified, not just the code change.

- threat model notes (assets, trust boundaries, abuse cases, mitigations)
- risk/control notes (what control exists, what it reduces, and how it’s verified)
- incident-style write-ups (timeline, evidence, impact, recommendations)

---

## What I’m Working On

- 🎓 **M.S. in Cybersecurity - Rider University**  
  Independent study focused on secure application engineering, with database security and AI security controls as major themes.

- 🤖 **CodePath Technical Fellow - AI 110**  
  I help students develop and debug Python and Streamlit apps with AI assistance. I also independently review the code from a security standpoint, identifying and remediating weaknesses to harden the applications in line with security frameworks.

- 🛰️ **Coastline College - STORM Cyber**  
  Part of Coastline’s STORM Cyber Career Development Program, combining cybersecurity coursework, certification prep, workshops, and Cybersecurity x AI research culminating in a paper, poster session, and research symposium.

- 📜 **CompTIA Network+**  
  Studying for CompTIA Network+ to strengthen hands-on networking knowledge in troubleshooting, configuration, management, and secure network support.

---

## Featured Projects

These are the first four projects in my current AI/security workstream. Across them, I’ve focused on input validation, safer error handling, auditability, rate limiting, financial integrity, and preserving evidence of security controls.

### 🐾 [PawPal+ - Pet Care Scheduler](https://github.com/JasmineSutton/PawPal)
*Python · Streamlit · OOP design*  
- centralized validation and allowlist checks at the backend boundary
- resource caps to prevent unbounded in-memory growth
- UI error handling to avoid leaking raw tracebacks
- atomic JSON saves + graceful handling of corrupted persistence
- automated tests (documented in security notes)

**Evidence:**  
- Security Notes: https://github.com/JasmineSutton/PawPal/blob/af862c49cb5f1e4ecc3b77f43fbd64890e68c3be/SECURITY_README.md  
- Case Study: https://github.com/JasmineSutton/JasmineSutton/blob/main/security-case-studies/pawpal-hardening.md

### 🍽️ [ByteBites - Secure Ordering Backend](https://github.com/JasmineSutton/ByteBites)
*Python · OOP design*  
- `Decimal`-based monetary calculations to avoid float rounding/precision risk
- strict allowlist validation + centralized sanitization helper
- explicit resource limits for catalog, transactions, and history
- immutable catalog returns to reduce shared mutable state risk
- tests + sanity check steps documented for verification

**Evidence:**  
- Security Notes: https://github.com/JasmineSutton/ByteBites/blob/7a449b0dfc703fabd62696536e602093e0feb620/SECURITY_README.md  
- Case Study: https://github.com/JasmineSutton/JasmineSutton/blob/main/security-case-studies/bytebites-hardening.md

### 🎮 [Game Glitch Investigator - Rate Limiting & Audit Trail](https://github.com/JasmineSutton/ai110-module1show-gameglitchinvestigator-starter)
*Python · Streamlit*  
- per-session rate limiting with cooldown and rolling-window controls
- structured event logging for security-relevant actions
- session-scoped audit trail with timestamps
- separation between interface behavior and core logic

**Evidence:**  
- Security Notes: https://github.com/JasmineSutton/Game-Glitch-Investigator/blob/main/Security.md
- Case Study: https://github.com/JasmineSutton/JasmineSutton/blob/main/security-case-studies/GameGlitchInvestigator-hardening.md

---

## What I’m Looking For

I’m open to full-time roles and projects in security engineering, governance, and application security where I can contribute to defensible systems, strong documentation, and practical security controls.

I’m especially drawn to teams working on secure system design, incident response, evidence-driven security work, and AI-related security problems.

---

## Tools and Areas I Work With

**Security / IR / Testing**  
Wireshark · Nmap · Burp Suite · Kali Linux · Metasploit · Nessus · FTK · Autopsy · Volatility · Gravwell

**Languages**  
Python · TypeScript · JavaScript · T-SQL · C · C++

**Cloud / Platforms**  
AWS · Azure · Azure DevOps

**Focus Areas**  
Security engineering · AI security · database security · governance · incident response · applied security research

---

## Certifications

ISC2 CC · Cisco Ethical Hacking · IBM Cybersecurity Professional Certificate · AWS ML Foundations · AWS Generative AI · Azure Fundamentals · CompTIA Network+ *(in progress)*

---

## Connect

Best way to reach me: Email  
Open to internships, projects, and full-time opportunities.  
📧 Jasmine.Sutton@live.com  

[![LinkedIn](https://img.shields.io/badge/LinkedIn-jasminesuttonit-blue?logo=linkedin)](https://linkedin.com/in/jasminesuttonit/)
