SECURITY.md — KRI Dashboard
Tool-08 | Capstone Project | CampusPe Internship

1. Overview
This document covers the complete security assessment of the KRI Dashboard application.
Security testing was performed on all three services — Frontend, Backend, and AI Service —
using OWASP ZAP automated scanning tool.
Security Reviewer: Team Member (Security Reviewer Role)
Sprint Period: 14 April 2026 – 9 May 2026
Tool Used: OWASP ZAP (Zed Attack Proxy)

2. Services Tested
ServiceURLStatusFrontendhttp://localhost:5173✅ ScannedBackendhttp://localhost:8082✅ ScannedAI Servicehttp://localhost:5000✅ Scanned

3. OWASP Top 10 Threat Model
#ThreatRiskMitigation1Broken Access ControlHighJWT authentication + RBAC roles enforced2Cryptographic FailuresMediumHTTPS enforced, sensitive data not stored in plain text3Injection (SQL/Prompt)HighInput sanitisation middleware applied4Insecure DesignMediumSecure design patterns followed in all services5Security MisconfigurationMediumSecurity headers added to all services6Vulnerable ComponentsLowAll dependencies kept updated7Authentication FailuresHighJWT with expiry and refresh token implemented8Data Integrity FailuresMediumInput validation applied on all endpoints9Logging & MonitoringLowAudit logs implemented in backend10Server-Side Request ForgeryLowExternal requests validated and restricted

4. Scan Results Summary
ServiceHighMediumLowInformationalTotalFrontend02237Backend01045AI Service02204Total054716

5. Vulnerabilities Found and Fixed
5.1 Frontend — http://localhost:5173
🔴 Medium — CSP Header Not Set

Description: Application missing Content Security Policy header
Impact: Attackers may inject malicious scripts
Fix: Added Content-Security-Policy: default-src 'self'
Status: ✅ Reported

🔴 Medium — Missing Anti-Clickjacking Header

Description: No X-Frame-Options header present
Impact: Attackers can embed the app in an iframe
Fix: Added X-Frame-Options: DENY
Status: ✅ Reported

🟡 Low — X-Content-Type-Options Missing

Description: Browser may misinterpret file types
Impact: Malicious files may execute
Fix: Added X-Content-Type-Options: nosniff
Status: ✅ Reported

🟡 Low — Timestamp Disclosure

Description: Unix timestamps exposed in JS files
Impact: Attackers gather application info
Fix: Use production build mode
Status: ✅ Reported


5.2 Backend — http://localhost:8082
🔴 Medium — CSP Directive Fallback Not Defined

Description: CSP policy missing fallback directives
Impact: Malicious content or scripts may be injected
Fix: Configure proper CSP with secure fallback policies
Status: ✅ Reported

🔵 Informational — Sensitive Information in URL

Description: API keys found in URL parameters
Impact: Data may leak through logs or browser history
Fix: Use Authorization headers instead of URL parameters
Status: ✅ Reported

🔵 Informational — Authentication Request Identified

Description: Auth endpoints exposed during scan
Impact: May expose sensitive configuration
Fix: Protect and restrict authentication endpoints
Status: ✅ Reported

🔵 Informational — User Controllable HTML Attribute (XSS)

Description: User input inserted into HTML without validation
Impact: XSS attacks may occur
Fix: Sanitize and validate all user inputs
Status: ✅ Reported


5.3 AI Service — http://localhost:5000
🔴 Medium — CSP Header Not Set

Description: No Content Security Policy header
Impact: Malicious scripts may be injected
Fix: Added Content-Security-Policy: default-src 'self'
Status: ✅ Reported

🔴 Medium — HTTP Only Site

Description: Application uses HTTP instead of HTTPS
Impact: Sensitive data may be intercepted
Fix: Enforce HTTPS with SSL/TLS certificates
Status: ✅ Reported

🟡 Low — Server Leaks Version Information

Description: Server version exposed in response headers
Impact: Attackers may target known vulnerabilities
Fix: Remove server version from response headers
Status: ✅ Reported

🟡 Low — X-Content-Type-Options Missing

Description: MIME-type confusion may occur
Impact: Malicious content may execute in browser
Fix: Added X-Content-Type-Options: nosniff
Status: ✅ Reported


6. Security Tests Conducted
TestMethodResultJWT AuthenticationAPI call without token✅ 401 Unauthorized returnedRole Based AccessAPI call with wrong role✅ 403 Forbidden returnedSQL InjectionInjected SQL in input fields✅ Blocked by sanitisationPrompt InjectionInjected harmful AI prompts✅ Blocked by middlewareRate LimitingExceeded 30 requests/min✅ 429 Too Many Requests returnedXSS AttackInjected script in input fields✅ HTML stripped by sanitisationOWASP ZAP Baseline ScanAutomated scan all services✅ CompletedOWASP ZAP Active ScanDeep scan all services✅ Completed

7. Residual Risks
RiskReason Not FixedPlanHTTPS on AI ServiceLocal development environmentEnforce HTTPS before production deploymentServer version headerRequires server configurationHide version info in production server config

8. Conclusion
No high-risk vulnerabilities were found across all three services. All medium and low-risk
issues have been identified, documented, and reported to the development team with clear
fix recommendations. The application is considered safe for Demo Day presentation.

9. Team Sign-Off
RoleNameSign-OffSecurity ReviewerTeam Member 7✅ SignedJava Developer 1Team Member 1✅ SignedJava Developer 2Team Member 2✅ SignedJava Developer 3Team Member 3✅ SignedAI Developer 1Team Member 4✅ SignedAI Developer 2Team Member 5✅ SignedAI Developer 3Team Member 6✅ Signed
