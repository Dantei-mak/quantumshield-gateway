# CONCEPT NOTE FOR PROJECT

**Candidate Name:** [Candidate Name] &nbsp;|&nbsp; **Reg. No:** [T/UDOM/202X/XXXXX] &nbsp;|&nbsp; **Supervisor:** [Supervisor Name]  
**Programme:** Bachelor of Science in Software Engineering  
**Department:** Department of Computer Science and Software Engineering  
**Institution:** College of Informatics and Virtual Education (CIVE), University of Dodoma (UDOM)  

---

### 1. Project Title:
A Biometric Secured Offline Mobile System for Tourist Safety and Monitoring in Tanzania

---

### 2. Background:
**2.1** Tourism is a critical economic pillar in Tanzania, generating foreign exchange and employment through world-renowned conservation destinations managed by the Tanzania National Parks Authority (TANAPA), such as Serengeti, Ngorongoro, and Mount Kilimanjaro (MNRT, 2023). However, the vast geographic expanse and rugged terrain of these parks create chronic telecommunication deficits and persistent cellular dead zones. Consequently, standard cloud-dependent emergency mobile applications fail completely within remote wilderness sectors.

**2.2** The absence of resilient digital safety mechanisms exposes tourists to severe hazards, including medical crises, wildlife encounters, and vehicle breakdowns without communication links (UNWTO, 2022). Furthermore, existing community reporting channels suffer from unverified submissions and hoax distress dispatches that strain limited park ranger resources. The lack of offline identity authentication and automated store-and-forward telemetry severely impedes the rapid, credible transmission of incident reports.

**2.3** To overcome these challenges, this project designs an offline-first mobile safety and monitoring application with on-device biometric verification before dispatching emergency SOS alerts or conservation reports. The solution incorporates local SQLite storage with automated background synchronization upon network restoration, lightweight on-device artificial intelligence for park regulations, basic Swahili translation, first aid, wildlife image classification, a conservation donation module, and a centralized web dashboard for ranger monitoring.

---

### 3. Objectives:
* **3.1 Main Objective:**  
  To design, implement, and evaluate a biometric-secured, offline-first mobile application integrated with a ranger administration web dashboard to enhance tourist safety, citizen conservation reporting, and wildlife monitoring across remote Tanzanian national parks.

* **3.2 Specific Objectives:**  
  1. To develop an offline-first mobile architecture utilizing native on-device biometric authentication to authenticate users before capturing geo-tagged conservation reports and SOS signals in local SQLite storage, with automatic synchronization upon cellular reconnection.  
  2. To integrate a lightweight on-device AI assistant for offline TANAPA park rules, basic Swahili translation, emergency first-aid guidance, wildlife image classification, and a mobile micro-donation interface for voluntary conservation funding.  
  3. To implement a web-based ranger administration dashboard for real-time visualization of incoming geo-tagged alerts, incident triage, and donation tracking, and to evaluate system synchronization reliability and usability under simulated network-deprived conditions.

---

### 4. Project Description:
* **Scope:** In-scope encompasses native device biometric authentication, local SQLite storage with auto-sync, on-device edge AI, micro-donations, and an administrative ranger web portal. Out-of-scope explicitly excludes live government database integration, custom external biometric servers, and commercial booking engines.
* **Target Audience:** International and domestic tourists, licensed safari tour guides, wildlife conservation field staff, and TANAPA park rangers/administrators.
* **Methodology:** Adopts an iterative Agile software engineering approach covering requirements modeling, mobile UI/database development, lightweight edge AI model integration, and laboratory network-interruption stress testing.
* **Timeline:** Phase 1: Requirements Analysis & Architecture (Weeks 1–3); Phase 2: Mobile App & Local SQLite Prototyping (Weeks 4–7); Phase 3: Edge AI, Sync Engine & Web Dashboard (Weeks 8–11); Phase 4: System Integration, Evaluation & Defense (Weeks 12–14).

---

### 5. Expected Outcomes:
A fully functional, offline-first Android mobile application; an operational ranger administration web dashboard; verified biometric verification and synchronization telemetry; and empirical evaluation benchmarks demonstrating eliminated false distress reports and accelerated emergency response times in remote conservation zones.
