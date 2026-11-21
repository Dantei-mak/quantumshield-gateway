# QuantumShield Gateway (PQ-Gateway)

> **Crypto-Agile, Post-Quantum Secure Financial Gateway & Inter-Bank Tunnel Engine**

[![Security Standard](https://img.shields.io/badge/NIST-FIPS%20203%20%7C%20FIPS%20204-blue)](https://csrc.nist.gov/Projects/post-quantum-cryptography)
[![Architecture](https://img.shields.io/badge/Architecture-Crypto--Agile%203--Layer-green)](docs/architecture/System_Architecture_and_UML_Design.md)
[![Compliance](https://img.shields.io/badge/Standards-ISO%2020022%20%7C%20PCI--DSS%20v4.0-orange)](docs/SRS/SRS_v1.0.md)

---

## 1. Overview
**QuantumShield Gateway** is an enterprise-grade, crypto-agile security gateway designed for financial institutions, central banks, and payment processors. It delivers quantum-resilient confidentiality and entity authentication across inter-bank communication links, protecting financial transactions against the imminent threat of **Harvest Now, Decrypt Later (HNDL)** quantum cryptanalysis.

The system incorporates finalized post-quantum standards:
- **NIST FIPS 203 (ML-KEM):** Module-Lattice-Based Key-Encapsulation Mechanism for quantum-safe session key establishment.
- **NIST FIPS 204 (ML-DSA):** Module-Lattice-Based Digital Signature Algorithm for mutual entity authentication and ISO 20022 payment authorization.
- **NIST FIPS 205 (SLH-DSA):** Stateless Hash-Based Digital Signature fallback.
- **Hybrid Key Exchange (RFC 9370 / X25519MLKEM768):** Defense-in-depth combining classical elliptic curves (X25519) with ML-KEM-768 via HKDF-SHA384.
- **Bulk Payload Encryption (AES-256-GCM):** High-throughput authenticated encryption with hardware acceleration.

---

## 2. Architecture & Design Principles
The platform implements a **Three-Layer Security Architecture** (Communication, Transaction, Infrastructure) governed by a **Crypto-Agile Service Abstraction (Provider Pattern)**. Banking applications communicate through clean abstract interfaces, enabling algorithm updates, key length adjustments, and compliance upgrades without restructuring underlying networks or core banking ledgers.

```
+-----------------------------------------------------------------------------------+
|                    CORE BANKING & FINANCIAL WORKLOAD LAYER                        |
|             (ISO 20022 camt.050 / pacs.008 Liquidity Messages)                    |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                     QUANTUMSHIELD CRYPTO-AGILE SERVICE GATEWAY                    |
|  +--------------------+   +---------------------+   +--------------------------+  |
|  | Classical Provider |   | Post-Quantum (PQC)  |   | Hybrid Combiner          |  |
|  | (X25519, RSA-4096) |   | (ML-KEM, ML-DSA)    |   | (HKDF-SHA384 RFC 5869)   |  |
|  +--------------------+   +---------------------+   +--------------------------+  |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                     AUTHENTICATED HIGH-SPEED OVERLAY TUNNEL                       |
|           (Point-to-Point Virtual Network 10.10.0.0/24 - AES-256-GCM)             |
+-----------------------------------------------------------------------------------+
```

---

## 3. Repository Structure
```
├── docs/                        # Formal specifications and technical designs
│   ├── SRS/                     # Software Requirements Specification (SRS v1.0)
│   └── architecture/            # UML models, network guides & system blueprints
├── papers/                      # Research foundation & BIS Project Leap reports
├── src/                         # Production Gateway source code
│   ├── config/                  # Declarative profiles & algorithm configuration
│   ├── crypto/                  # Crypto-Agile Provider engine (Classical, PQC, Hybrid)
│   ├── gateway/                 # Asynchronous client & server tunnel services
│   ├── payloads/                # ISO 20022 XML financial payment processors
│   └── telemetry/               # Audit-grade telemetry, metrics & benchmarks
├── tests/                       # Security verification & fault-injection test suites
└── README.md
```

---

## 4. Getting Started

### Prerequisites
- Python 3.10+
- OpenSSL 3.x (with ML-KEM and ML-DSA support)
- Git

### Initial Setup
```bash
# Clone the repository
git clone https://github.com/Dantei-mak/<repo-name>.git
cd <repo-name>

# Verify OpenSSL capabilities
openssl list -kem-algorithms
openssl list -signature-algorithms | grep ML-DSA
```

---

## 5. Security & Commercial Licensing
All rights reserved. Designed for commercial deployment in financial infrastructures.
For enterprise licensing, pilots, and architecture assessments, contact the core development team.
