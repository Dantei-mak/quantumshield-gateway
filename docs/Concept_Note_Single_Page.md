# CONCEPT NOTE

**Project Title:** Hybrid Post-Quantum Secure VPN for Inter-Bank Communication  
**Programme:** Bachelor of Science in Software Engineering  
**Department:** Department of Computer Science and Software Engineering  
**Institution:** College of Informatics and Virtual Education (CIVE), University of Dodoma (UDOM)  
**Candidate Name:** [Candidate Name] | **Reg. No:** [T/UDOM/202X/XXXXX] | **Supervisor:** [Supervisor Name]  

---

### 1. Introduction

Inter-bank settlement infrastructures rely critically on site-to-site Virtual Private Networks (VPNs) to ensure the confidentiality and integrity of high-value financial transactions. Real-Time Gross Settlement (RTGS) systems and central bank gateways transmit standardized ISO 20022 payment messages—including liquidity transfers and credit instructions—over encrypted tunnels established using classical asymmetric cryptography such as RSA-2048 and Elliptic Curve Diffie-Hellman (ECDH). Because these financial clearing records settle values exceeding national GDP daily, preserving their long-term privacy and authenticity is vital to national economic stability.

However, rapid progress in quantum computing poses an existential threat to these classical security architectures. A Cryptanalytically Relevant Quantum Computer running Shor’s algorithm can solve integer factorization and discrete logarithms in polynomial time, completely compromising RSA and elliptic curve protocols. Furthermore, adversaries are actively conducting "Harvest Now, Decrypt Later" (HNDL) attacks by intercepting and archiving encrypted banking traffic today. Because sovereign financial records carry confidentiality lifespans of several decades, historical transaction data recorded today will be decrypted retroactively once quantum hardware matures.

To counter this vulnerability, the National Institute of Standards and Technology (NIST) finalized post-quantum standards in August 2024, notably FIPS 203 (ML-KEM) for key encapsulation and FIPS 204 (ML-DSA) for digital signatures. However, abrupt wholesale replacement introduces operational risks, including 30x to 50x larger key footprints that cause packet fragmentation across standard network links. A hybrid approach—combining classical X25519 with post-quantum ML-KEM-768 via HKDF key derivation—provides defense-in-depth, guaranteeing session security as long as either mathematical foundation remains unbroken.

---

### 2. Objectives

#### 2.1 Main Objective
To design, implement, and evaluate a hybrid post-quantum secure Virtual Private Network that provides quantum-resilient communication between two simulated financial institutions while systematically quantifying the performance and architectural trade-offs of post-quantum migration.

#### 2.2 Specific Objectives
1. **Design and Architecture:** To design an isolated laboratory network topology and a hybrid cryptographic protocol combining classical ECDH (X25519) and NIST FIPS 203 (ML-KEM-768) for secure inter-bank data exchange.
2. **Implementation and Authentication:** To implement the hybrid VPN gateway with NIST FIPS 204 (ML-DSA-65) certificate-based mutual endpoint authentication and an AES-256-GCM encrypted data plane.
3. **Empirical Benchmarking:** To measure and compare handshake latency, CPU and memory utilization, network throughput, and packet fragmentation overhead across Classical, Pure PQC, and Hybrid modes using synthetic ISO 20022 payment workloads.

---

### 3. Conclusion

This project delivers a practical, quantum-safe communication blueprint tailored for modern financial networks navigating the post-quantum transition. By demonstrating that a hybrid PQC-VPN can mitigate Harvest Now, Decrypt Later threats without disrupting underlying transport protocols or degrading bulk throughput, the study provides valuable empirical benchmarks for financial institutions, network engineers, and regulatory bodies preparing for secure quantum-era migration.
