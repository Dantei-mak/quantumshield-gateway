# CONCEPT NOTE

**1.0 Project Title:** A BIOMETRIC SECURED OFFLINE MOBILE SYSTEM FOR TOURIST SAFETY AND MONITORING IN TANZANIA  
**Programme:** Bachelor of Science in Software Engineering  
**Department:** Department of Computer Science and Software Engineering  
**Institution:** College of Informatics and Virtual Education (CIVE), University of Dodoma (UDOM)  
**Candidate Name:** [Candidate Name] &nbsp;|&nbsp; **Reg. No:** [T/UDOM/202X/XXXXX] &nbsp;|&nbsp; **Supervisor:** [Supervisor Name]  

---

### 2.0 BACKGROUND AND INTRODUCTION

**2.1** Tourism serves as a cornerstone of Tanzania’s socio-economic development, generating significant foreign exchange earnings and employment through world-renowned conservation destinations managed by the Tanzania National Parks Authority (TANAPA), such as Serengeti, Ngorongoro, and Mount Kilimanjaro (Ministry of Natural Resources and Tourism [MNRT], 2023). However, the vast geographic expanse and rugged terrain of these protected areas present severe telecommunication infrastructure deficits, creating persistent cellular dead zones for tourists and wildlife researchers. Consequently, conventional cloud-reliant emergency and safety mobile applications become completely dysfunctional precisely when assistance is most critical.

**2.2** The absence of resilient digital safety mechanisms exposes tourists to heightened vulnerabilities, including dangerous wildlife encounters, medical crises, and vehicle breakdowns without reliable communication channels (World Tourism Organization [UNWTO], 2022). Furthermore, existing community reporting workflows suffer from unverified submissions and hoax distress dispatches that strain limited park ranger resources. The lack of offline-first identity verification and automated store-and-forward telemetry severely impedes the rapid, credible transmission of incident reports and conservation observations.

**2.3** To resolve these operational challenges, this project designs an offline-first mobile safety and monitoring system featuring on-device biometric verification (utilizing native hardware authentication) before dispatching emergency SOS alerts or conservation reports. The solution incorporates local SQLite storage with automated background synchronization upon network restoration, lightweight edge artificial intelligence for offline park regulations, basic Swahili translation, first-aid assistance, wildlife image classification, a voluntary conservation donation module, and a centralized web dashboard for ranger monitoring (without interfacing live government servers or commercial booking engines).

---

### 3.0 OBJECTIVES OF THE PROJECT

#### 3.1 Main Objective
To design, implement, and evaluate a biometric-secured, offline-first mobile application integrated with a ranger administration web dashboard to enhance tourist safety, citizen conservation reporting, and wildlife monitoring across remote Tanzanian national parks.

#### 3.2 Specific Objectives
1. **Biometric Security & Offline Data Synchronization:** To develop an offline-first mobile architecture utilizing native on-device biometric authentication (fingerprint/facial recognition) to authenticate users prior to capturing geo-tagged conservation reports and SOS distress signals in local SQLite storage, with automatic background synchronization upon cellular network reconnection.
2. **Lightweight On-Device AI & Micro-Donation:** To integrate a lightweight, on-device AI assistant providing offline natural park rules, basic Swahili conversational guidance, first-aid decision support, basic wildlife image classification, and a mobile donation interface to facilitate voluntary conservation funding.
3. **Ranger Administrative Dashboard & System Evaluation:** To implement a centralized web-based ranger administration dashboard for real-time visualization of incoming geo-tagged alerts, incident triage, and donation tracking, and to evaluate system latency, synchronization reliability, and usability under simulated network-deprived conditions.

---

### 4.0 CONCLUSION

In conclusion, this project provides a vital, technically grounded software engineering intervention tailored to the unique connectivity constraints of Tanzanian national parks. By integrating local biometric verification, robust offline-to-online data synchronization, edge AI guidance, and an intuitive ranger dashboard, the system mitigates false distress alerts and accelerates emergency response while strictly adhering to manageable project scope boundaries. Ultimately, the system establishes a dependable digital safety net that bolsters sustainable eco-tourism and wildlife preservation across Tanzania.
