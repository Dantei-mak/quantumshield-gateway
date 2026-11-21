# FINAL YEAR PROJECT CONCEPT NOTE

**Institution:** The University of Dodoma (UDOM)  
**College:** College of Informatics and Virtual Education (CIVE)  
**Department:** Department of Computer Science and Software Engineering  
**Programme:** Bachelor of Science in Software Engineering  
**Academic Year:** 2025/2026  
**Candidate Name:** `[Candidate Name]`  
**Registration Number:** `[T/UDOM/202X/XXXXX]`  
**Proposed Supervisor:** `[Supervisor Name]`  
**Deliverable Stage:** Concept Note (1st – 3rd Week)  

---

## 1.0 Project Title
**Hybrid Post-Quantum Secure VPN for Inter-Bank Communication**

---

## 2.0 Background and Introduction

### 2.1 Classical VPN Dependency in Financial Market Infrastructures
Inter-bank settlement systems rely critically on classical public-key cryptography to secure cross-institutional Virtual Private Network (VPN) communications across untrusted networks. Financial Market Infrastructures (FMIs), central banks, and Real-Time Gross Settlement (RTGS) gateways utilize asymmetric algorithms—specifically Rivest–Shamir–Adleman (`RSA-2048`/`4096`), Elliptic Curve Diffie–Hellman (`ECDH Curve25519`), and the Elliptic Curve Digital Signature Algorithm (`ECDSA P-256`)—to authenticate institutional endpoints and establish ephemeral session keys. Across these encrypted tunnels, banking institutions exchange standardized `ISO 20022` financial messages, including `camt.050` liquidity transfers and `pacs.008` credit instructions, which settle transaction values exceeding national Gross Domestic Product (GDP) weekly (International Organization for Standardization [ISO], 2022; Bank for International Settlements [BIS] Innovation Hub, 2023). Consequently, the integrity and long-term confidentiality of inter-bank VPN handshakes represent a foundational pillar of national and global financial stability.

### 2.2 Quantum Cryptanalysis and the "Harvest Now, Decrypt Later" Threat
Rapid advancements in quantum computing threaten to render classical asymmetric cryptography obsolete before financial institutions complete infrastructure migration. A Cryptanalytically Relevant Quantum Computer (CRQC) executing Shor's algorithm can solve both integer factorization and discrete logarithm problems in polynomial time, completely breaking `RSA`, `DH`, `ECDH`, and `ECDSA` handshakes. Although symmetric ciphers such as `AES-256-GCM` remain quantum-resilient because Grover's algorithm provides only a quadratic speedup (reducing effective security to $2^{128}$ operations), hostile actors are actively conducting **"Harvest Now, Decrypt Later" (HNDL)** espionage by intercepting and archiving encrypted financial VPN traffic today for retrospective decryption once quantum hardware matures. According to Mosca's (2021) quantum risk theorem ($X + Y > Z$), when the required confidentiality lifespan of sovereign banking records ($X$, typically 10–30 years) plus the infrastructure migration duration ($Y$, 5–10 years) exceeds the time to quantum maturation ($Z$), classical inter-bank traffic transmitted today is already compromised.

### 2.3 Post-Quantum Standardization and Central Bank Transition Initiatives
Newly finalized international post-quantum standards provide the mathematical foundation required to build quantum-resistant financial communication tunnels. In August 2024, the U.S. National Institute of Standards and Technology (NIST) published its first three Post-Quantum Cryptography (PQC) Federal Information Processing Standards: **FIPS 203 (`ML-KEM`)** for module-lattice key encapsulation, **FIPS 204 (`ML-DSA`)** for module-lattice digital signatures, and **FIPS 205 (`SLH-DSA`)** for stateless hash-based signatures (NIST, 2024a, 2024b, 2024c). Leading central banks have begun evaluating these algorithms in experimental environments, notably through the BIS Innovation Hub *Project Leap* Phase 1 and Phase 2 trials conducted with the Banque de France, Deutsche Bundesbank, Bank of Italy, and Swift (BIS Innovation Hub, 2023, 2025). Nevertheless, transitioning production-grade banking VPNs to these standards introduces complex architectural and network performance trade-offs that require systematic engineering investigation.

---

## 3.0 Statement of the Problem

### 3.1 Cryptographic Obsolescence vs. Operational Risks of Pure-PQC Migration
Financial institutions face an acute migration dilemma between quantum-vulnerable classical VPNs and operationally disruptive pure post-quantum replacements. Retaining legacy classical VPN configurations exposes high-value `ISO 20022` settlement messages to Shor's algorithm and active HNDL archival (Mosca, 2021). Conversely, abruptly replacing classical primitives with pure PQC algorithms introduces severe network overhead because lattice-based keys, ciphertexts, and signatures are **30× to 50× larger** than their elliptic-curve counterparts. Specifically, whereas classical `X25519` exchanges `32-byte` keys and `ECDSA P-256` generates `64-byte` signatures, `ML-KEM-768` requires a `1,184-byte` public key and `1,088-byte` ciphertext, and `ML-DSA-65` produces a `1,952-byte` public key and `3,309-byte` signature (NIST, 2024a, 2024b). These multi-kilobyte artifacts exceed the standard `1,500-byte` Ethernet Maximum Transmission Unit (MTU), causing IP packet fragmentation, elevated handshake latency, increased CPU/RAM consumption, and single-point reliance on newer lattice assumptions without classical fallback assurance (BIS Innovation Hub, 2025).

### 3.2 Absence of Reproducible Hybrid Benchmarks and Cryptographic Agility
Banking network engineers—particularly across developing economies—lack an empirically benchmarked, cryptographically agile hybrid VPN framework to quantify and mitigate these migration risks prior to production deployment. Legacy VPN implementations tightly couple network transport sockets to hardcoded cipher suites, preventing administrators from dynamically switching algorithms or adjusting security parameters if a primitive is weakened. While high-budget central bank trials such as *Project Leap* demonstrate initial feasibility using specialized infrastructure (BIS Innovation Hub, 2023, 2025), regional financial institutions and academic researchers lack a reproducible software testbed that systematically compares Classical, Pure PQC, and Hybrid VPN configurations across standardized `ISO 20022` workloads. Without empirical data on handshake overhead multipliers, packet fragmentation counts, and bulk throughput parity, financial institutions cannot safely plan post-quantum capacity upgrades.

### 3.3 Proposed Solution Overview
To resolve this problem, this project designs, implements, and empirically evaluates a **Cryptographically Agile Hybrid Post-Quantum VPN (Hybrid PQC-VPN)** across an isolated dual-institution laboratory testbed connecting an initiating financial institution (`macOS Host`, `10.10.0.2`) and a responding settlement gateway (`Ubuntu Linux VM`, `10.10.0.1`). The architecture executes classical `X25519` key exchange in parallel with `NIST FIPS 203 (ML-KEM-768)` via `OpenSSL 3.x` and `liboqs` (Stebila & Mosca, 2024), combining both shared secrets through `RFC 5869 HKDF-SHA384` to key an `AES-256-GCM` data plane—ensuring session confidentiality holds as long as *at least one* mathematical family remains unbroken. Furthermore, the system enforces mutual endpoint authentication using `NIST FIPS 204 (ML-DSA-65)` X.509 certificates, decouples cryptographic primitives behind an abstract provider interface (`ICryptoProvider`) for zero-code agility across NIST Security Categories 1, 3, and 5, and executes $N = 50$ automated benchmark trials per mode using synthetic `ISO 20022` (`camt.050` and `pacs.008`) workloads (ISO, 2022).

---

## 4.0 Main Objective of the Project
To design, implement, and empirically evaluate a hybrid post-quantum secure Virtual Private Network that provides quantum-resilient communication between two simulated financial institutions while systematically quantifying the performance, computational, and architectural impacts of post-quantum migration.

---

## 5.0 Specific Objectives
- **5.1 Specific Objective 1 (Architectural Design):** To design an isolated laboratory network architecture and hybrid cryptographic protocol model representing secure communication between two autonomous financial institutions.
- **5.2 Specific Objective 2 (Classical Baseline Implementation):** To implement a standards-compliant classical cryptographic VPN configuration (using `RSA`/`ECDH` and `AES-256-GCM`) that establishes a baseline for security and performance benchmarking.
- **5.3 Specific Objective 3 (Hybrid & PQC Implementation):** To implement a hybrid post-quantum VPN architecture combining classical primitives with `NIST FIPS 203 (ML-KEM)` for key encapsulation and `NIST FIPS 204 (ML-DSA)` for endpoint authentication.
- **5.4 Specific Objective 4 (Empirical Performance Evaluation):** To evaluate and benchmark tunnel establishment duration, handshake latency, CPU/RAM utilization, network throughput, and packet fragmentation overhead across Classical, Pure PQC, and Hybrid configurations under variable `ISO 20022` financial workloads.
- **5.5 Specific Objective 5 (Cryptographic Agility Assessment):** To evaluate the system's cryptographic agility by testing dynamic algorithm switching, parameter adjustment (NIST Security Levels 1, 3, and 5), and algorithmic fault tolerance without restructuring the underlying VPN transport layer.

---

## 6.0 Significance and Expected Benefits of the Project

### 6.1 Immediate Mitigation of "Harvest Now, Decrypt Later" Threats in Banking
The proposed Hybrid PQC-VPN provides financial institutions with an immediate, standards-compliant defense-in-depth mechanism against retrospective quantum decryption. By combining classical elliptic-curve key agreement (`X25519`) with module-lattice key encapsulation (`ML-KEM-768`) inside an `HKDF-SHA384` combiner, the architecture ensures that an adversary must simultaneously break both classical discrete logarithms and post-quantum lattice problems to recover session keys (NIST, 2024a; Stebila & Mosca, 2024). Consequently, central and commercial banks can protect long-lived `ISO 20022` liquidity and credit settlement payloads (`camt.050` and `pacs.008`) against HNDL espionage today while preserving full compliance with existing classical security mandates (ISO, 2022; Mosca, 2021).

### 6.2 Empirical Capacity-Planning Dataset for Financial IT Infrastructure
The project's automated benchmarking framework delivers quantitative decision-support data that de-risks post-quantum network migration for banking IT architects. Through statistically rigorous experimentation ($N = 50$ trials per mode reporting mean, median, standard deviation, and 95th percentile metrics), the study quantifies exact handshake latency multipliers, CPU and resident memory (RSS) overhead, bulk `AES-256-GCM` throughput parity, and `1,500-byte` MTU IP packet fragmentation induced by multi-kilobyte `ML-KEM` and `ML-DSA` artifacts (NIST, 2024a, 2024b). Financial network engineers can directly utilize these empirical profiles to size gateway hardware and tune TCP/MTU parameters before modifying live RTGS production environments (BIS Innovation Hub, 2025).

### 6.3 Reusable Cryptographic Agility Blueprint and Regional Readiness
Beyond empirical metrics, the software artifact establishes a reusable cryptographic agility blueprint and an accessible post-quantum research testbed for Tanzanian and African financial institutions. By encapsulating cryptographic operations behind an abstract provider interface (`ICryptoProvider`), the design decouples VPN network transport from underlying mathematical primitives—enabling zero-code hot-swapping across NIST Security Categories 1, 3, and 5 (`ML-KEM-512`/`768`/`1024`, `ML-DSA-44`/`65`/`87`, and backup `FIPS 205 SLH-DSA`) via declarative configuration files (NIST, 2024a, 2024b, 2024c). Furthermore, translating multi-million-dollar central bank experiments such as BIS *Project Leap* into a self-contained VirtualBox laboratory at the University of Dodoma (UDOM) empowers regional banks and future researchers to validate quantum-safe financial protocols on standard computing hardware (BIS Innovation Hub, 2023, 2025).

---

## 7.0 Preliminary References
1. Bank for International Settlements Innovation Hub, Banque de France, & Deutsche Bundesbank. (2023). *Project Leap: Quantum-proofing the financial system (Phase 1 Report)*. Bank for International Settlements.
2. Bank for International Settlements Innovation Hub, Bank of Italy, Banque de France, Deutsche Bundesbank, Nexi-Colt, & Swift. (2025). *Project Leap: Quantum-proofing payment systems (Phase 2 Report)*. Bank for International Settlements.
3. International Organization for Standardization. (2022). *ISO 20022 Financial services — Universal financial industry message scheme*. ISO.
4. Mosca, M. (2021). Cybersecurity in an era with quantum computers: Will we be ready? *IEEE Security & Privacy*, 16(5), 38–41.
5. National Institute of Standards and Technology. (2024a). *Module-lattice-based key-encapsulation mechanism standard (ML-KEM)* (FIPS Publication 203). U.S. Department of Commerce.
6. National Institute of Standards and Technology. (2024b). *Module-lattice-based digital signature standard (ML-DSA)* (FIPS Publication 204). U.S. Department of Commerce.
7. National Institute of Standards and Technology. (2024c). *Stateless hash-based digital signature standard (SLH-DSA)* (FIPS Publication 205). U.S. Department of Commerce.
8. Stebila, D., & Mosca, M. (2024). *Post-quantum key exchange for the Internet and the Open Quantum Safe project (liboqs & oqs-provider for OpenSSL 3.x)*. https://openquantumsafe.org
