# Research Proposal: QRFL Framework

**Full Title:** QRFL: A Byzantine-Resistant Post-Quantum Federated Learning Framework for Edge Deployment

**Author:** [Your Name]  
**Institution:** [Your Institution]  
**Date:** December 30, 2025  
**Duration:** 4 months  
**Research Type:** Framework Development + Experimental Validation

---

## Executive Summary

This proposal presents **QRFL (Quantum-Resistant Federated Learning)**, the first production-ready framework for Byzantine-resistant post-quantum secure federated learning validated on edge devices. While existing research has explored post-quantum cryptography (PQC) in federated learning (FL), all prior work remains in the research prototype stage—untested on edge hardware, lacking Byzantine resilience, or too complex for practical deployment. QRFL bridges the critical gap between academic research and real-world usability by providing a modular, pip-installable framework that enables developers to deploy quantum-resistant FL systems with a single command.

**Key Innovation:** We unify Byzantine fault tolerance, post-quantum security, and edge optimization in a single practical framework, validated on Raspberry Pi hardware—something no existing work has achieved.

---

## 1. Introduction

### 1.1 Background

**Federated Learning (FL)** enables collaborative machine learning across distributed participants without centralizing raw data, addressing privacy concerns in healthcare, finance, and IoT applications. However, two critical threats undermine FL's security:

1. **Quantum Attacks:** Current FL systems rely on classical cryptography (RSA, ECDH) vulnerable to Shor's algorithm. The "harvest-now-decrypt-later" threat means adversaries can store today's encrypted FL communications and decrypt them once quantum computers mature (~2030-2035).

2. **Byzantine Attacks:** Malicious participants can poison FL models through gradient manipulation, model replacement, or targeted data attacks, compromising model integrity.

**Post-Quantum Cryptography (PQC)** offers quantum-resistant algorithms (Kyber, Dilithium, Falcon) standardized by NIST in 2024. However, integrating PQC into FL is non-trivial due to:
- Large key/ciphertext sizes (10-100× classical)
- Computational overhead on resource-constrained devices
- Protocol redesign for multi-round FL workflows

### 1.2 The Critical Gap

Our comprehensive literature review of 7 recent papers (2022-2025) reveals:

| Work | PQC | Byzantine | Edge Validated | Usable Framework |
|------|-----|-----------|----------------|------------------|
| LaF (Xu et al., 2022) | ✅ | ❌ | ❌ | ❌ |
| Yang et al. (2022) | ✅ | ❌ | ❌ | ❌ |
| PQSF (2025) | ✅ | ❌ | ❌ | ❌ |
| V2G Auth (2025) | ✅ | ✅ | ❌ | ❌ |
| PQBFL (2025) | ✅ | ✅ | ❌ | ❌ |
| Quantum FL (2024) | ⚠️ | ❌ | ❌ | ❌ |
| PQC Testing (2025) | ✅ | ❌ | ❌ | ❌ |
| **QRFL (Proposed)** | **✅** | **✅** | **✅** | **✅** |

**Key Findings:**
- **Zero frameworks** combine Byzantine resilience + PQC + edge validation
- **Zero frameworks** are pip-installable or production-ready
- Yang et al. (2022) explicitly cite communication overhead as "open question"
- PQBFL (2025) achieves Byzantine+PQC but with excessive complexity (5+ security layers) and no edge validation
- All existing work tested on high-end hardware (Intel i9, H100 GPU) not representative of real-world edge deployments

### 1.3 Research Motivation

**Why This Matters:**

1. **Quantum Timeline:** NIST PQC standards published (2024), migration window narrowing
2. **Edge Computing Growth:** 75 billion IoT devices by 2025 (IDC), FL adoption accelerating
3. **Practical Need:** Healthcare, finance, government require HIPAA/GDPR-compliant quantum-safe FL
4. **Developer Friction:** Existing FL frameworks (Flower, PySyft) lack PQC support; researchers spend months implementing from scratch

**Market Gap:** No production-ready quantum-resistant FL framework exists despite clear demand.

---

## 2. Problem Statement

### 2.1 Core Challenges

**Challenge 1: Security Trilemma**
- **Quantum Resistance:** Requires PQC with large keys/ciphertexts
- **Byzantine Tolerance:** Requires signature verification, anomaly detection
- **Edge Efficiency:** Resource-constrained devices (Raspberry Pi: 4GB RAM, ARM CPU)

Existing solutions optimize 1-2 dimensions but sacrifice the third.

**Challenge 2: Communication Overhead**
- PQC keys: Kyber512 public key = 800 bytes (vs RSA-2048 = 256 bytes)
- Multi-round FL: N participants × M rounds = N×M key exchanges
- Yang et al. (2022): Quadratic communication scaling (acknowledged as open problem)

**Challenge 3: Heterogeneity**
- Devices range from Raspberry Pi (4 cores, 4GB RAM) to servers (64 cores, 128GB RAM)
- No existing work provides adaptive protocols for diverse capabilities
- One-size-fits-all approaches either over-burden weak devices or under-utilize strong ones

**Challenge 4: Byzantine Detection Complexity**
- PQBFL (2025): Dual ratcheting + blockchain + PUF + ZKP = impractical for edge
- V2G (2025): FL-based anomaly detection = high computational cost
- Need: Lightweight cryptographic verification suitable for edge devices

**Challenge 5: Developer Adoption**
- Existing work: Research prototypes requiring weeks to replicate
- No pip-installable packages, minimal documentation, no maintenance
- Gap between "published paper" and "tool developers use"

### 2.2 Research Questions

**RQ1:** Can we design a Byzantine-resistant post-quantum secure aggregation protocol with **sub-quadratic communication complexity** suitable for edge devices?

**RQ2:** What is the **minimal security layer set** sufficient for Byzantine resistance without the complexity of PQBFL's 5-layer architecture?

**RQ3:** How do we enable **heterogeneity-aware adaptive protocols** that dynamically adjust to device capabilities without manual configuration?

**RQ4:** Can quantum-resistant FL achieve **acceptable performance** (≤3× classical overhead) on Raspberry Pi 4 hardware?

**RQ5:** What framework design enables **production-ready deployment** (pip install, 5-minute setup, comprehensive docs)?

---

## 3. Research Objectives

### 3.1 Primary Objectives

**O1: Framework Design**
Develop QRFL, a modular Byzantine-resistant post-quantum FL framework with:
- Pluggable PQC algorithms (Kyber, Dilithium, Falcon)
- Configurable Byzantine detection mechanisms
- Framework-agnostic ML support (PyTorch, TensorFlow, JAX)

**O2: Protocol Optimization**
Design a 3-round secure aggregation protocol that:
- Reduces communication overhead vs Yang et al. (2022) baseline
- Achieves linear (not quadratic) scaling with dropout participants
- Supports multi-round key reuse with forward secrecy

**O3: Edge Validation**
Validate QRFL on Raspberry Pi 4 cluster (3 devices) with:
- Real network conditions (WiFi, variable latency)
- Resource monitoring (CPU, memory, power consumption)
- Performance comparison vs simulated environments

**O4: Production Readiness**
Deliver a usable framework with:
- PyPI package (`pip install qrfl`)
- CLI tool for quick start
- Comprehensive documentation and tutorials
- Automated testing and CI/CD

### 3.2 Success Criteria

| Criterion | Target | Baseline |
|-----------|--------|----------|
| Communication overhead | ≤1.5× Yang et al. | Yang et al. = 2.5× SecAgg |
| Round latency (RPi) | ≤5 seconds | Unknown (no prior edge work) |
| Byzantine detection rate | ≥95% | PQBFL = 97% (simulated) |
| Memory footprint | ≤500 MB | Flower = 380 MB (classical) |
| Setup time | ≤5 minutes | Existing research = hours/days |
| Documentation coverage | 100% API + 5 tutorials | Prior work = minimal README |

---

## 4. Proposed Approach

### 4.1 Framework Architecture

QRFL consists of **6 modular layers**:

```
┌─────────────────────────────────────────────────────┐
│          Layer 6: Application Interface            │
│  (PyTorch/TensorFlow/JAX Model Integration)        │
├─────────────────────────────────────────────────────┤
│       Layer 5: Heterogeneity Adaptation            │
│  (Device Profiling, Adaptive Protocol Selection)   │
├─────────────────────────────────────────────────────┤
│        Layer 4: Byzantine Detection                │
│  (Signature Verification, Anomaly Detection)       │
├─────────────────────────────────────────────────────┤
│       Layer 3: Secure Aggregation Protocol         │
│  (Single Masking, SHPRG, Secret Sharing)           │
├─────────────────────────────────────────────────────┤
│     Layer 2: Multi-Round Key Management            │
│  (Symmetric Ratcheting, Periodic Key Refresh)      │
├─────────────────────────────────────────────────────┤
│    Layer 1: Post-Quantum Secure Channel            │
│       (Kyber KEM, Dilithium Signatures)            │
└─────────────────────────────────────────────────────┘
```

**Design Principles:**
- **Modularity:** Each layer independently testable and replaceable
- **Simplicity:** Avoid PQBFL's over-engineering (no blockchain, no PUF)
- **Efficiency:** Optimized for edge constraints
- **Extensibility:** Plugin system for new PQC algorithms

### 4.2 Technical Components

#### 4.2.1 Layer 1: Post-Quantum Secure Channel

**Primitives:**
- **Key Exchange:** Kyber-768 (NIST Level 3 security, 128-bit quantum)
- **Signatures:** Dilithium3 (balance of size vs speed)
- **Symmetric Encryption:** AES-256-GCM for data confidentiality

**Protocol:**
```
Client → Server: Kyber public key (1184 bytes)
Server → Client: Kyber ciphertext (1088 bytes) + Dilithium signature
Client ↔ Server: AES-GCM encrypted communications (shared secret K)
```

**Rationale:**
- Kyber: Fastest NIST KEM, smallest ciphertext among finalists
- Dilithium: Smaller signatures than Falcon (2420 vs 666 bytes) but faster signing
- AES-GCM: Post-quantum symmetric encryption (quantum algorithms don't break symmetric crypto)

#### 4.2.2 Layer 2: Multi-Round Key Management

**Challenge:** Avoid N² key exchanges every round (Yang et al. overhead)

**Solution: Symmetric Ratcheting**
```python
# Initial: Kyber KEM establishes root key K₀
K₀ = Kyber.Decaps(kyber_ciphertext, private_key)

# Each round: Derive new session key
K_round[i] = KDF(K_round[i-1], round_number || context)

# Periodic refresh (every 10 rounds): New Kyber exchange
if round % 10 == 0:
    K₀ = Kyber.Decaps(new_ciphertext, private_key)
```

**Properties:**
- Forward secrecy: Compromise of K_i doesn't reveal K_{i-1}
- Efficiency: KDF faster than Kyber (microseconds vs milliseconds)
- Simplicity: No dual ratcheting complexity (PQBFL)

**Comparison:**
- PQBFL: Symmetric + asymmetric dual ratcheting (complex)
- Ours: Symmetric only with periodic refresh (simpler, sufficient)

#### 4.2.3 Layer 3: Secure Aggregation Protocol

**Based on Yang et al. (2022) single masking**, with optimizations:

**3-Round Protocol:**

**Round 1: Setup & Key Exchange**
```
Client → Server: Kyber public key, device capabilities
Server → Clients: Kyber ciphertexts, participant list P
Result: All participants share keys, logical ordering established
```

**Round 2: Seed Sharing**
```
Each client i:
  1. Generate SHPRG seed: s_i ← {0,1}^λ
  2. Split seed: shares ← SSS.Share(s_i, t=⌈2N/3⌉, n=N)
  3. Encrypt shares: c_{i→j} ← AES-GCM(K_{i,j}, share_j)
  4. Send to server: {c_{i→j}}_{j∈P}

Server:
  Relay encrypted shares (cannot decrypt, doesn't learn seeds)
```

**Round 3: Masked Model Upload & Aggregation**
```
Each client i:
  1. Train local model: x_i ← LocalTrain(data_i, global_model)
  2. Generate mask: r_i ← SHPRG(s_i)
  3. Encrypt model: y_i ← x_i + r_i
  4. Sign: σ_i ← Dilithium.Sign(y_i || round_num)
  5. Upload: (y_i, σ_i)

Server:
  1. Verify signatures: ∀i, Dilithium.Verify(σ_i, y_i, pk_i)
  2. Recover seed sum: S_sum ← SSS.Recover({shares from P_3})
  3. Compute mask sum: R_sum ← SHPRG(S_sum)
  4. Unmask: z ← Σy_i - R_sum = Σx_i + Σr_i - R_sum ≈ Σx_i
  5. Update global model: global_model ← z / |P_3|
```

**Optimization vs Yang et al.:**
- **Batch signature verification:** Verify N signatures in parallel (SIMD)
- **Incremental aggregation:** Start aggregating as clients upload (don't wait for all)
- **Compressed shares:** Encode shares with polynomial compression (save 30% bandwidth)

**Dropout Handling:**
- Threshold t = ⌈2N/3⌉: Up to ⌊N/3⌋ clients can drop without aborting
- Independent of dropout count (Yang et al.'s key advantage over SecAgg)

#### 4.2.4 Layer 4: Byzantine Detection

**Two-Tier Approach:**

**Tier 1: Cryptographic Verification (Mandatory)**
```python
# Every round, every client
for client_i in participants:
    if not Dilithium.Verify(signature_i, model_i, pubkey_i):
        reject(client_i)  # Signature forgery detected
        blacklist(client_i, duration=10_rounds)
```

**Tier 2: Statistical Anomaly Detection (Optional)**
```python
# Gradient norm checking
median_norm = median([norm(x_i) for x_i in models])
for x_i in models:
    if norm(x_i) > 3 * median_norm:  # Outlier threshold
        flag_suspicious(client_i)
        if repeated_violations(client_i):
            blacklist(client_i)
```

**Rationale:**
- Tier 1: Lightweight, cryptographically sound, catches forgery
- Tier 2: Catches poisoning attacks not detectable cryptographically
- Unlike V2G: Not FL-based (no expensive model training for detection)
- Unlike PQBFL: No blockchain overhead

**Byzantine Attack Coverage:**
| Attack Type | Detection Method | Overhead |
|-------------|------------------|----------|
| Gradient forgery | Signature verification | Low |
| Model poisoning | Statistical anomaly | Medium |
| Replay attack | Nonce/round number | Negligible |
| Sybil attack | Identity registration | One-time |

#### 4.2.5 Layer 5: Heterogeneity Adaptation

**Problem:** Raspberry Pi vs Desktop have 10× compute difference

**Solution: Adaptive Protocol Variants**

**Device Profiling (Round 1):**
```python
profile = {
    "cpu_cores": 4,
    "ram_mb": 4096,
    "cpu_type": "ARM Cortex-A72",
    "network_bandwidth_mbps": 50,
    "estimated_crypto_ops_per_sec": 1200  # Benchmark
}
server.classify_device(profile) → "LOW" | "MEDIUM" | "HIGH"
```

**Protocol Variants:**

| Device Class | PQC Level | Signature Scheme | Batch Size | Timeout |
|--------------|-----------|------------------|------------|---------|
| LOW (RPi)    | Kyber512  | Dilithium2       | 10 models  | 30s     |
| MEDIUM       | Kyber768  | Dilithium3       | 50 models  | 15s     |
| HIGH         | Kyber1024 | Dilithium5       | 100 models | 10s     |

**Dynamic Adjustment:**
```python
# If client consistently times out
if client.timeout_rate > 0.3:
    client.protocol_variant = downgrade(client.protocol_variant)
    client.timeout *= 1.5
```

**Benefit:** Low-end devices participate without slowing down high-end devices

#### 4.2.6 Layer 6: Application Interface

**Developer API:**

```python
# qrfl/client.py
from qrfl import QRFLClient, Config
import torch

config = Config.from_yaml("config.yaml")  # or Config(pqc_algorithm="kyber768", ...)
client = QRFLClient(server_url="localhost:8080", config=config)

# Train with your PyTorch model
model = YourNeuralNetwork()
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)

client.train(
    model=model,
    train_data=local_dataset,
    epochs=5,
    optimizer=optimizer
)
```

```python
# qrfl/server.py
from qrfl import QRFLServer, Config

config = Config(
    pqc_algorithm="kyber768",
    signature_algorithm="dilithium3",
    byzantine_threshold=0.3,  # Tolerate 30% malicious clients
    min_clients=5,
    aggregation_rounds=100
)

server = QRFLServer(model=global_model, config=config)
server.start()  # Blocks until training complete
```

**CLI Tool:**
```bash
# Quick start
qrfl init my-fl-project --template mnist
cd my-fl-project
qrfl start --server &
qrfl start --clients 5

# Deploy to Raspberry Pi
qrfl deploy rpi --hosts pi1.local,pi2.local,pi3.local --config config.yaml

# Run benchmarks
qrfl benchmark --suite standard --output results.json
```

### 4.3 Comparison with Existing Work

| Aspect | Yang et al. (2022) | PQBFL (2025) | QRFL (Ours) |
|--------|-------------------|--------------|-------------|
| **Rounds** | 3 | 2 | 3 |
| **Masking** | Single (SHPRG) | Dual ratcheting | Single (optimized) |
| **Key Management** | Basic Shamir | Symmetric+Asymmetric | Symmetric ratcheting |
| **Byzantine** | ❌ | ✅ (5 layers) | ✅ (2 tiers) |
| **Communication** | O(N²) acknowledged | O(N²) + blockchain | O(N log N) target |
| **Edge Validated** | ❌ (Intel i9) | ❌ | ✅ (Raspberry Pi) |
| **Heterogeneity** | ❌ | ❌ | ✅ |
| **Usability** | Prototype | Prototype | Production framework |
| **Complexity** | Medium | Very High | Medium-Low |

---

## 5. Implementation Plan

### 5.1 Technology Stack

**Programming Language:** Python 3.10+
- Rationale: ML ecosystem compatibility, rapid prototyping

**Cryptography:**
- **liboqs-python:** NIST PQC algorithms (Kyber, Dilithium, Falcon)
- **cryptography:** AES-GCM, KDF, secure random

**Networking:**
- **gRPC:** Efficient binary protocol, HTTP/2 multiplexing
- **Protocol Buffers:** Serialization

**ML Frameworks:**
- **PyTorch:** Primary support (most popular for research)
- **TensorFlow:** Secondary support
- **JAX:** Future work

**Testing:**
- **pytest:** Unit and integration tests
- **pytest-benchmark:** Performance regression tests
- **Docker:** Reproducible environments

**Documentation:**
- **Sphinx:** API documentation
- **MkDocs:** User guides and tutorials
- **Jupyter:** Interactive examples

### 5.2 Development Phases

#### **Phase 1: Core Cryptography (Weeks 1-2)**

**Tasks:**
- [ ] Setup project structure (qrfl/, tests/, docs/, examples/)
- [ ] Implement PQC layer (Kyber KEM wrapper)
- [ ] Implement signature layer (Dilithium wrapper)
- [ ] Implement SHPRG (Learning with Rounding PRG)
- [ ] Implement Shamir Secret Sharing
- [ ] Unit tests for all cryptographic primitives

**Deliverables:**
- `qrfl/crypto/` module with 100% test coverage
- Benchmark results: Kyber/Dilithium performance on x86 and ARM

**Validation:**
```bash
pytest tests/crypto/ --cov=qrfl/crypto
python benchmarks/crypto_benchmark.py --platform rpi4
```

#### **Phase 2: Secure Aggregation Protocol (Weeks 2-4)**

**Tasks:**
- [ ] Implement 3-round protocol state machine
- [ ] Client-side: Key exchange, seed sharing, model upload
- [ ] Server-side: Key distribution, share relay, aggregation
- [ ] Dropout handling with threshold secret sharing
- [ ] gRPC service definitions

**Deliverables:**
- `qrfl/protocol/` module
- Integration tests: 5-client simulation on single machine
- Protocol specification document

**Validation:**
```bash
pytest tests/protocol/ --cov=qrfl/protocol
python tests/integration/test_full_round.py
```

#### **Phase 3: Byzantine Detection (Weeks 4-5)**

**Tasks:**
- [ ] Tier 1: Signature verification pipeline
- [ ] Tier 2: Statistical anomaly detection (gradient norm, cosine similarity)
- [ ] Blacklist management (temporary bans, reputation scoring)
- [ ] Attack simulation framework

**Deliverables:**
- `qrfl/security/` module
- Byzantine attack test suite (gradient poisoning, model replacement)
- Security evaluation report

**Validation:**
```python
# Simulate 30% Byzantine clients
python tests/security/test_byzantine_attacks.py --malicious-ratio 0.3
# Expected: Detection rate > 95%
```

#### **Phase 4: Edge Validation (Weeks 5-7)**

**Tasks:**
- [ ] Setup Raspberry Pi cluster (3 devices)
- [ ] Deploy QRFL server (laptop) + 3 QRFL clients (RPi)
- [ ] Run MNIST/CIFAR-10 federated training
- [ ] Measure: latency, throughput, CPU/memory/power consumption
- [ ] Compare vs simulated environment

**Deliverables:**
- Raspberry Pi deployment scripts
- Performance dataset: 100+ training rounds
- Edge validation report

**Hardware Setup:**
```
Laptop (Server):
  - Intel i7-10750H, 16GB RAM
  - Role: Aggregation server

Raspberry Pi 4 Model B × 3 (Clients):
  - ARM Cortex-A72, 4GB RAM
  - Connected via WiFi (802.11ac)
  - Role: Local training + secure upload
```

**Experiments:**
| Experiment | Clients | Rounds | Dataset | Metrics |
|------------|---------|--------|---------|---------|
| E1: Baseline | 3 | 50 | MNIST | Latency, accuracy |
| E2: Dropout | 3 (1 drops) | 50 | MNIST | Resilience |
| E3: Byzantine | 3 (1 malicious) | 50 | MNIST | Detection rate |
| E4: Scalability | 3-10 (simulated) | 100 | CIFAR-10 | Throughput |

#### **Phase 5: Heterogeneity & Optimization (Weeks 7-9)**

**Tasks:**
- [ ] Implement device profiling
- [ ] Implement adaptive protocol variants
- [ ] Batch signature verification
- [ ] Parallel aggregation pipeline
- [ ] Multi-round key management

**Deliverables:**
- `qrfl/adaptive/` module
- Performance optimization report (before/after)
- Heterogeneity evaluation (RPi + Desktop mixed)

**Validation:**
```python
# Mixed deployment: 2 RPi (low) + 1 Desktop (high)
python tests/heterogeneity/test_mixed_deployment.py
# Expected: Desktop not bottlenecked by RPi
```

#### **Phase 6: Framework Packaging (Weeks 9-12)**

**Tasks:**
- [ ] CLI tool development (`qrfl` command)
- [ ] Configuration system (YAML/JSON)
- [ ] Quick start templates (MNIST, CIFAR-10, text classification)
- [ ] API documentation (Sphinx)
- [ ] User tutorials (5+ Jupyter notebooks)
- [ ] CI/CD pipeline (GitHub Actions)
- [ ] PyPI packaging

**Deliverables:**
- PyPI package: `pip install qrfl`
- Documentation website: qrfl.readthedocs.io
- GitHub repository with README, examples, contribution guide

**Validation:**
```bash
# Fresh Ubuntu VM
pip install qrfl
qrfl quickstart mnist
cd mnist && qrfl start
# Expected: Training starts within 5 minutes
```

#### **Phase 7: Evaluation & Paper Writing (Weeks 12-16)**

**Tasks:**
- [ ] Comprehensive benchmarks (communication, computation, memory)
- [ ] Comparison with Yang et al., PQBFL baselines
- [ ] Security analysis and threat modeling
- [ ] Write paper sections (Introduction, Related Work, Design, Evaluation)
- [ ] Generate figures and tables
- [ ] Internal review and revisions

**Deliverables:**
- Full paper draft (12-15 pages)
- Benchmark dataset (public release)
- Camera-ready submission

### 5.3 Raspberry Pi Experimental Setup

**Hardware:**
- 3× Raspberry Pi 4 Model B (4GB RAM)
- 1× Laptop (Ubuntu 22.04, server role)
- WiFi router (802.11ac)

**Software:**
- Raspberry Pi OS (64-bit)
- Python 3.10, liboqs, PyTorch (ARM builds)
- Monitoring: psutil, powermetrics

**Network Topology:**
```
Internet
   |
[Router] ──┐
           ├── [Laptop: Server]
           ├── [RPi-1: Client]
           ├── [RPi-2: Client]
           └── [RPi-3: Client]
```

**Monitoring Script:**
```python
# monitor.py - Log resource usage every second
import psutil, time

while training:
    log({
        "cpu_percent": psutil.cpu_percent(),
        "memory_mb": psutil.virtual_memory().used / 1e6,
        "network_sent_mb": psutil.net_io_counters().bytes_sent / 1e6,
        "network_recv_mb": psutil.net_io_counters().bytes_recv / 1e6,
        "timestamp": time.time()
    })
    time.sleep(1)
```

---

## 6. Evaluation Strategy

### 6.1 Metrics

**Security Metrics:**
- Byzantine detection rate (%)
- False positive rate (%)
- Attack success rate (should be 0%)

**Performance Metrics:**
- Round latency (seconds)
- Communication overhead (MB per round)
- Computation time (client-side, server-side)
- Memory footprint (MB)
- Power consumption (Watts, RPi only)

**Usability Metrics:**
- Setup time (minutes from `pip install` to first training)
- Lines of code required (user perspective)
- Documentation coverage (% of API documented)

### 6.2 Baseline Comparisons

**Baseline 1: Classical Flower**
- Represents standard FL without quantum resistance
- Metrics: Performance upper bound (no PQC overhead)

**Baseline 2: Yang et al. (2022) - Simulated**
- Replicate their protocol in our environment
- Metrics: Communication overhead, round latency
- Goal: Demonstrate our optimizations reduce overhead

**Baseline 3: PQBFL (2025) - Simulated**
- Simplified implementation of dual ratcheting approach
- Metrics: Complexity (LOC), performance
- Goal: Show we achieve Byzantine resistance with less complexity

### 6.3 Experimental Scenarios

**Scenario 1: Honest Participants**
- All clients follow protocol
- Measures: Best-case performance

**Scenario 2: Byzantine Attacks**
- Gradient poisoning: 30% clients send 10× gradients
- Model replacement: 30% clients send random models
- Measures: Detection rate, model accuracy impact

**Scenario 3: Dropout Resilience**
- Random client dropouts (10%, 20%, 30% per round)
- Measures: Round completion rate, latency impact

**Scenario 4: Heterogeneous Deployment**
- Mixed: 2 RPi (low) + 2 Desktop (high)
- Measures: Load balancing, adaptive protocol effectiveness

**Scenario 5: Scalability**
- Vary clients: 5, 10, 20, 50, 100 (simulated beyond 3 RPi)
- Measures: Communication scaling, aggregation time

### 6.4 Datasets & Models

| Dataset | Task | Model | Clients | Data Split |
|---------|------|-------|---------|------------|
| MNIST | Digit classification | CNN (50K params) | 10 | IID |
| MNIST | Digit classification | CNN | 10 | Non-IID (2 digits/client) |
| CIFAR-10 | Image classification | ResNet-18 (11M params) | 20 | IID |
| IMDB | Sentiment analysis | LSTM (1M params) | 10 | IID |

**Rationale:**
- MNIST: Lightweight, fast iteration for debugging
- CIFAR-10: Realistic image task, larger model
- IMDB: Demonstrate NLP compatibility
- IID vs Non-IID: Evaluate robustness to data heterogeneity

### 6.5 Threat Model Validation

**Adversary Capabilities (As per Threat Model):**
- Semi-honest server: Follows protocol but tries to infer private data
- Semi-honest clients: Fraction (≤30%) try to poison model
- Network adversary: Passive eavesdropping, MITM attempts

**Security Tests:**
| Attack | Method | Expected Defense |
|--------|--------|------------------|
| Eavesdropping | Packet capture (Wireshark) | Kyber-encrypted, no plaintext leakage |
| MITM | ARP spoofing | Signature verification detects tampering |
| Gradient inference | Analyze aggregated model | Single masking prevents isolation |
| Model poisoning | Send 10× gradients | Statistical anomaly detection flags |
| Replay attack | Resend old messages | Nonce/round number mismatch |
| Quantum attack | Post-quantum simulator | PQC algorithms resist (by design) |

---

## 7. Expected Contributions

### 7.1 Novel Research Contributions

**C1: First Byzantine-Resistant PQC FL Framework for Edge Devices**
- **Novelty:** No existing work validates PQC FL on edge hardware (all use desktops/servers)
- **Impact:** Demonstrates real-world feasibility, not just theoretical possibility

**C2: Communication-Optimized Secure Aggregation Protocol**
- **Novelty:** Addresses Yang et al.'s "open question" on communication overhead
- **Techniques:** Batch verification, compressed secret shares, incremental aggregation
- **Impact:** Sub-quadratic scaling vs O(N²) baseline

**C3: Heterogeneity-Aware Adaptive Protocol Design**
- **Novelty:** First PQC FL work addressing device heterogeneity
- **Approach:** Device profiling + adaptive protocol variants
- **Impact:** Enables participation of weak devices without bottlenecking strong ones

**C4: Practical Byzantine Detection for PQC FL**
- **Novelty:** Simpler than PQBFL (2 tiers vs 5 layers), more robust than V2G (cryptographic + statistical)
- **Impact:** Balances security and complexity for real deployment

**C5: Production-Ready Framework Design**
- **Novelty:** First pip-installable quantum-resistant FL framework
- **Impact:** Accelerates research and adoption (like Flower did for classical FL)

### 7.2 Practical Contributions

**PC1: Open-Source Framework**
- Public GitHub repository with MIT/Apache license
- PyPI package: `pip install qrfl`
- Active maintenance and community support

**PC2: Comprehensive Documentation**
- API reference (100% coverage)
- 5+ tutorials (beginner to advanced)
- Deployment guides (cloud, edge, hybrid)

**PC3: Benchmark Suite**
- Standardized datasets and metrics
- Reproducible experiment configs
- Public leaderboard for comparisons

**PC4: Educational Resources**
- Jupyter notebooks explaining PQC+FL concepts
- Video tutorials
- Workshop materials

### 7.3 Broader Impact

**For Researchers:**
- Accelerate PQC FL research (no need to reimplement basics)
- Enable focus on ML aspects, not cryptography

**For Industry:**
- Provide production-ready solution for quantum-safe FL
- Enable HIPAA/GDPR-compliant deployments

**For Education:**
- Teaching tool for quantum cryptography + distributed ML
- Hands-on labs with real edge devices

**For Standardization:**
- Reference implementation for future PQC FL standards
- Protocol specification for interoperability

---

## 8. Timeline & Milestones

### 8.1 Gantt Chart

```
Month 1:
Week 1: [████████] Phase 1: Core Cryptography
Week 2: [████████] Phase 1 Complete
Week 3: [████████] Phase 2: Protocol (Part 1)
Week 4: [████████] Phase 2: Protocol (Part 2)

Month 2:
Week 5: [████████] Phase 2 Complete + Phase 3: Byzantine Detection
Week 6: [████████] Phase 4: Edge Validation (Setup)
Week 7: [████████] Phase 4: Edge Validation (Experiments)

Month 3:
Week 8: [████████] Phase 4 Complete
Week 9: [████████] Phase 5: Heterogeneity & Optimization
Week 10: [████████] Phase 6: Framework Packaging (Part 1)
Week 11: [████████] Phase 6: Framework Packaging (Part 2)
Week 12: [████████] Phase 6 Complete

Month 4:
Week 13: [████████] Phase 7: Comprehensive Evaluation
Week 14: [████████] Phase 7: Paper Writing (Intro, Related Work, Design)
Week 15: [████████] Phase 7: Paper Writing (Evaluation, Discussion)
Week 16: [████████] Phase 7: Paper Finalization & Submission
```

### 8.2 Key Milestones

| Milestone | Week | Deliverable | Success Criterion |
|-----------|------|-------------|-------------------|
| M1: Crypto Module | 2 | qrfl/crypto/ complete | 100% test coverage |
| M2: Protocol Implementation | 4 | 3-round protocol working | 5-client simulation successful |
| M3: Byzantine Detection | 5 | Security module complete | >95% detection rate |
| M4: Edge Validation | 7 | RPi experiments complete | Round latency <5s |
| M5: Optimization | 9 | Heterogeneity support | Mixed deployment working |
| M6: Framework Release | 12 | PyPI package published | `pip install qrfl` works |
| M7: Paper Submission | 16 | Full paper draft | Ready for conference submission |

### 8.3 Risk Mitigation

**Risk 1: Raspberry Pi Performance Insufficient**
- **Probability:** Medium
- **Impact:** High (undermines edge validation claim)
- **Mitigation:** 
  - Early performance testing (Week 2)
  - Fallback: Use more powerful edge devices (Jetson Nano)
  - Optimization: ARM-specific assembly (NEON SIMD)

**Risk 2: Communication Overhead Still Quadratic**
- **Probability:** Low
- **Impact:** Medium (reduces novelty vs Yang et al.)
- **Mitigation:**
  - Multiple optimization strategies (batch verification, compression)
  - Incremental improvements acceptable (1.5× vs 2.5× Yang et al.)

**Risk 3: Byzantine Detection Rate <95%**
- **Probability:** Low
- **Impact:** Medium
- **Mitigation:**
  - Two-tier approach provides redundancy
  - Threshold tuning based on experiments
  - Comparison with PQBFL's 97% (simulated) still valid

**Risk 4: Timeline Overrun**
- **Probability:** Medium
- **Impact:** High
- **Mitigation:**
  - 2-week buffer built into 4-month plan
  - Phased delivery (framework usable by Week 12, paper by Week 16)
  - MVP approach: Core features first, nice-to-haves optional

---

## 9. Resource Requirements

### 9.1 Hardware

| Item | Quantity | Purpose | Cost (Est.) |
|------|----------|---------|-------------|
| Raspberry Pi 4 (4GB) | 3 | Edge clients | $55 × 3 = $165 |
| MicroSD Card (64GB) | 3 | RPi storage | $10 × 3 = $30 |
| USB-C Power Supply | 3 | RPi power | $8 × 3 = $24 |
| Laptop | 1 (owned) | Server + development | $0 |
| WiFi Router | 1 (owned) | Networking | $0 |
| **Total** | | | **$219** |

### 9.2 Software

| Item | License | Cost |
|------|---------|------|
| Python, PyTorch, etc. | Open source | Free |
| liboqs (PQC library) | MIT | Free |
| VS Code, Git | Open source | Free |
| GitHub Actions | Free tier (sufficient) | Free |
| Read the Docs | Open source projects | Free |
| **Total** | | **$0** |

### 9.3 Cloud Resources (Optional)

| Service | Purpose | Cost (Est./month) |
|---------|---------|-------------------|
| AWS EC2 t3.medium | Scalability testing | $30 |
| GitHub Storage (LFS) | Large dataset hosting | $5 |
| **Total** | (Optional, 2 months) | **$70** |

### 9.4 Total Budget

**Required:** $219 (hardware only)
**Optional:** +$70 (cloud resources)
**Grand Total:** $289

---

## 10. Evaluation & Validation Plan

### 10.1 Correctness Validation

**Unit Tests:**
- 200+ test cases covering all modules
- Edge cases: Dropout, timeout, invalid input
- Property-based testing (Hypothesis library)

**Integration Tests:**
- End-to-end FL training scenarios
- Multi-client simulations (5, 10, 20 clients)
- Failure injection (network partitions, client crashes)

**Security Tests:**
- Cryptographic correctness (NIST test vectors)
- Attack simulations (6 attack types)
- Penetration testing (external security audit if budget allows)

### 10.2 Performance Validation

**Benchmarks:**
```python
# tests/performance/benchmark_suite.py
def test_kyber_keygen_latency():
    assert measure_latency(Kyber.KeyGen) < 1.0  # milliseconds

def test_round_latency_5_clients():
    assert measure_round(clients=5) < 3.0  # seconds

def test_communication_overhead():
    classical_size = measure_flower_communication()
    qrfl_size = measure_qrfl_communication()
    assert qrfl_size < 2.0 * classical_size  # <2× overhead
```

**Regression Testing:**
- CI/CD runs benchmarks on every commit
- Alert if performance degrades >10%

### 10.3 Usability Validation

**Developer Testing:**
- 5 external developers (grad students) attempt quick start
- Measure: Time to first successful training
- Target: <10 minutes (stretch: <5 minutes)

**Documentation Review:**
- External reviewer reads docs, finds missing sections
- Iterate until 90% of common questions answered

**API Ergonomics:**
- Compare lines of code vs Flower for same task
- Target: ≤2× code complexity

---

## 11. Ethical Considerations

### 11.1 Dual-Use Concerns

**Potential Misuse:** Secure aggregation could enable privacy-preserving training of harmful models (e.g., deepfakes, misinformation)

**Mitigation:**
- Documentation includes "Ethical Use" section
- Recommend content moderation at application layer
- Note: Ethical considerations same as classical FL (PQC doesn't change this)

### 11.2 Environmental Impact

**Energy Consumption:** PQC operations consume more power than classical

**Mitigation:**
- Measure power consumption (RPi experiments)
- Provide energy-efficient mode (Kyber512 instead of Kyber1024)
- Carbon offset recommendations for large deployments

### 11.3 Accessibility

**Digital Divide:** Requires edge devices (RPi ~$60)

**Mitigation:**
- Support for older hardware (RPi 3B+)
- Cloud client option (browser-based via WASM, future work)
- Documentation for resource-constrained scenarios

---

## 12. Dissemination Plan

### 12.1 Academic Venues

**Target Conferences (Tier 1):**
- ACM CCS (Computer and Communications Security)
- USENIX Security Symposium
- IEEE S&P (Security and Privacy)
- NDSS (Network and Distributed System Security)

**Submission Timeline:**
- March 2026: ACM CCS (deadline typically May)
- Alternative: August 2026: USENIX Security (deadline typically February)

**Workshop/Poster Venues:**
- PPML (Privacy Preserving Machine Learning) @ NeurIPS
- FL-ICML (Federated Learning workshop @ ICML)

### 12.2 Open Source Release

**GitHub Repository:** github.com/[username]/qrfl
- Public from Day 1 (transparent development)
- MIT or Apache 2.0 license
- Issue tracker, discussions, wiki

**PyPI Release:**
- Alpha: Week 10 (limited testing)
- Beta: Week 12 (stable API)
- Stable 1.0: Post-paper acceptance

**Documentation Site:**
- qrfl.readthedocs.io or custom domain
- Built with Sphinx + MkDocs
- Auto-generated API docs

### 12.3 Community Engagement

**Blog Posts:**
- Medium/personal blog series (6 posts):
  1. "Why PQC FL Matters"
  2. "Designing QRFL: Architecture Decisions"
  3. "Byzantine Detection in PQC FL"
  4. "Edge Deployment: Lessons from Raspberry Pi"
  5. "Framework Design: From Research to Production"
  6. "QRFL 1.0 Release Announcement"

**Conference Talks:**
- Submit to PyData, PyCon (if accepted)
- University seminar series

**Social Media:**
- Twitter/X: Development updates
- Reddit: r/MachineLearning, r/cryptography
- Hacker News: Launch announcement

---

## 13. Success Metrics

### 13.1 Research Success

| Metric | Target | Measurement |
|--------|--------|-------------|
| Paper acceptance | Tier 1 conference | Peer review outcome |
| Citation impact | 10+ citations (Year 1) | Google Scholar |
| Novel contribution | 5 distinct contributions | As outlined in Section 7 |
| Edge validation | RPi round latency <5s | Experimental measurement |

### 13.2 Framework Success

| Metric | Target | Measurement |
|--------|--------|-------------|
| GitHub stars | 100+ (Year 1) | GitHub metrics |
| PyPI downloads | 500+ (Year 1) | PyPI stats |
| Test coverage | >90% | pytest-cov |
| Documentation coverage | 100% API | Sphinx metrics |
| Issue response time | <48 hours | GitHub average |

### 13.3 Community Success

| Metric | Target | Measurement |
|--------|--------|-------------|
| External contributors | 5+ | GitHub contributor graph |
| Stack Overflow Q&A | 20+ questions | SO tag monitoring |
| Tutorial completions | 100+ | Google Analytics (docs) |
| Conference workshops | 1+ accepted | CFP responses |

---

## 14. Related Work (Detailed)

### 14.1 Classical Secure Aggregation

**SecAgg (Bonawitz et al., 2017)**
- 4-round MPC protocol with double masking
- Pairwise Diffie-Hellman for mask generation
- Limitation: Quadratic communication, quantum-vulnerable

**SecAgg+ (Bell et al., 2020)**
- k-regular graph topology (vs complete graph)
- Polylogarithmic complexity
- Limitation: Still DH-based, complex topology

**FastSecAgg (Kadhe et al., 2021)**
- FFT-based secret sharing
- Limitation: Low dropout tolerance (<10%), not PQC

### 14.2 Post-Quantum Secure Aggregation

**LaF (Xu et al., 2022)**
- First lattice-based FL framework
- Multi-use secret sharing
- Limitation: Double masking overhead, no Byzantine, no edge validation

**Yang et al. (2022)** ← Primary comparison baseline
- Single masking with SHPRG (LWR)
- 3 rounds, Kyber KEM
- Limitation: Quadratic communication (acknowledged), semi-honest only, high-end hardware

**PQSF (2025)**
- 20% improvement over LaF
- Multiple secret sharing variants
- Limitation: Still quadratic, desktop simulation, no Byzantine

### 14.3 Byzantine-Resilient FL

**V2G Authentication (2025)**
- FL-based Byzantine detection
- Domain-specific (vehicle-to-grid)
- Limitation: Too complex, not general-purpose, no edge validation

**PQBFL (2025)** ← Byzantine comparison baseline
- Dual ratcheting + blockchain
- PUF + zero-knowledge proofs
- Limitation: Over-engineered (5 layers), no edge validation, high gas costs

**Krum, Bulyan, etc. (2017-2020)**
- Gradient-based Byzantine detection
- Limitation: Classical crypto, computationally expensive

### 14.4 Heterogeneous FL

**FedProx (Li et al., 2020)**
- Handles device heterogeneity via proximal term
- Limitation: No security focus, classical crypto

**HierFL (Luo et al., 2022)**
- Hierarchical aggregation for heterogeneous networks
- Limitation: Not quantum-resistant

### 14.5 Our Positioning

**Novelty Matrix:**

| Work | PQC | Byzantine | Edge | Heterogeneity | Usable |
|------|-----|-----------|------|---------------|--------|
| LaF | ✅ | ❌ | ❌ | ❌ | ❌ |
| Yang | ✅ | ❌ | ❌ | ❌ | ❌ |
| PQBFL | ✅ | ✅ | ❌ | ❌ | ❌ |
| V2G | ✅ | ✅ | ❌ | ⚠️ | ❌ |
| FedProx | ❌ | ❌ | ⚠️ | ✅ | ✅ |
| **QRFL** | **✅** | **✅** | **✅** | **✅** | **✅** |

**Gap Summary:** We are the first to achieve all five dimensions simultaneously.

---

## 15. Potential Challenges & Contingencies

### 15.1 Technical Challenges

**Challenge:** PQC overhead too high for RPi
**Contingency:** 
- Switch to Kyber512 (lighter variant)
- Optimize with ARM NEON intrinsics
- Accept higher overhead as cost of quantum resistance

**Challenge:** Byzantine detection rate <90%
**Contingency:**
- Tune thresholds based on experiments
- Add third detection tier (model accuracy voting)
- Compare favorably vs PQBFL (simulated) regardless

**Challenge:** Communication overhead not reduced vs Yang
**Contingency:**
- Reframe as "validated on edge" (still novel)
- Future work: Advanced compression techniques
- Honest reporting: X% improvement vs baseline

### 15.2 Timeline Challenges

**Challenge:** Development takes longer than 12 weeks
**Contingency:**
- MVP approach: Core features in Weeks 1-10
- Paper writing parallel to Weeks 10-12 development
- Defer nice-to-haves (CLI polish, extra tutorials)

**Challenge:** Paper rejected from first venue
**Contingency:**
- Incorporate reviews, resubmit to second venue
- Publish on arXiv immediately (establish priority)
- Framework still valuable independent of paper

### 15.3 Resource Challenges

**Challenge:** Raspberry Pi supply shortage
**Contingency:**
- Use alternatives: Jetson Nano, Orange Pi
- Reduce to 2 clients (still demonstrates edge)
- Simulated edge experiments (slower CPU setting)

---

## 16. Long-Term Vision

### 16.1 Post-Submission Roadmap

**Version 1.1 (6 months post-launch):**
- Falcon signature support (faster than Dilithium)
- TensorFlow integration (beyond PyTorch)
- Docker deployment automation

**Version 2.0 (1 year post-launch):**
- WebAssembly client (browser-based FL)
- Kubernetes operator (cloud deployment)
- Differential privacy integration

**Version 3.0 (18 months post-launch):**
- Hybrid quantum-classical support (future hardware)
- Cross-silo FL (beyond cross-device)
- Commercial support options

### 16.2 Research Extensions

**Follow-up Papers:**
1. "QRFL for Medical Imaging: A Case Study in Federated Healthcare"
2. "Optimizing PQC FL Communication with Neural Compression"
3. "QRFL-WASM: Quantum-Resistant FL in the Browser"

**PhD Thesis Potential:** QRFL as foundational Chapter 3, with extensions in Chapters 4-6

### 16.3 Community Growth

**Year 1:** Establish user base (500+ downloads)
**Year 2:** Community contributions (10+ external PRs)
**Year 3:** Industry adoption (3+ companies using in production)

---

## 17. Conclusion

This proposal presents **QRFL**, a comprehensive solution to the critical gap in quantum-resistant federated learning. By unifying Byzantine resilience, post-quantum security, edge validation, and production-ready design, QRFL advances the state-of-the-art across four dimensions simultaneously—something no existing work achieves.

**Key Strengths:**

1. **Addresses Real Problem:** "Harvest-now-decrypt-later" threat is imminent; developers need tools now
2. **Novel Research:** First Byzantine-resistant PQC FL validated on edge devices
3. **Practical Impact:** pip-installable framework accelerates adoption
4. **Rigorous Evaluation:** 3-device Raspberry Pi cluster provides real-world data
5. **Feasible Timeline:** 4 months with clear phases and milestones

**Expected Outcomes:**

- **Academic:** Tier 1 conference paper with 5 novel contributions
- **Practical:** Open-source framework used by researchers and industry
- **Community:** Foundation for future PQC FL standardization

**Broader Impact:** QRFL enables secure, quantum-resistant federated learning for healthcare (HIPAA), finance (fraud detection), government (national security), and IoT (edge AI)—domains where data privacy and long-term security are critical.

We are at a pivotal moment: NIST PQC standards are new (2024), quantum threats are accelerating, and FL adoption is exploding. QRFL seizes this opportunity to define the next generation of secure federated learning.

---

## 18. References

1. Bonawitz, K., et al. (2017). "Practical Secure Aggregation for Privacy-Preserving Machine Learning." ACM CCS.

2. Xu, P., et al. (2022). "LaF: Lattice-Based and Communication-Efficient Federated Learning." IEEE TIFS.

3. Yang, S., et al. (2022). "A Post-quantum Secure Aggregation for Federated Learning." ICCNS.

4. [PQBFL] (2025). "Post-Quantum Byzantine Federated Learning with Ratcheting Mechanisms."

5. [V2G Auth] (2025). "A Post-Quantum Secure Federated Learning Framework for Cross-Domain V2G Authentication."

6. NIST (2024). "Post-Quantum Cryptography Standardization." https://csrc.nist.gov/projects/post-quantum-cryptography

7. McMahan, B., et al. (2017). "Communication-Efficient Learning of Deep Networks from Decentralized Data." AISTATS.

8. Li, T., et al. (2020). "Federated Optimization in Heterogeneous Networks." MLSys.

9. Blanchard, P., et al. (2017). "Machine Learning with Adversaries: Byzantine Tolerant Gradient Descent." NeurIPS.

10. Open Quantum Safe Project. "liboqs: C library for quantum-resistant cryptographic algorithms." https://openquantumsafe.org/

---

## Appendix A: Detailed Architecture Diagrams

### A.1 System Architecture

```
┌───────────────────────────────────────────────────────────────┐
│                         QRFL System                            │
├───────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐         ┌─────────────┐                      │
│  │   Client 1  │         │   Server    │                      │
│  │  (RPi/PC)   │◄────────┤ Aggregator  │                      │
│  └─────────────┘  gRPC   └─────────────┘                      │
│                      │                                          │
│  ┌─────────────┐    │    ┌──────────────────┐                │
│  │   Client 2  │◄───┼────┤  Kyber KEM       │                │
│  │  (RPi/PC)   │    │    │  Key Exchange    │                │
│  └─────────────┘    │    └──────────────────┘                │
│                      │                                          │
│  ┌─────────────┐    │    ┌──────────────────┐                │
│  │   Client N  │◄───┴────┤  Secret Sharing  │                │
│  │  (RPi/PC)   │         │  Aggregation     │                │
│  └─────────────┘         └──────────────────┘                │
│                                                                 │
└───────────────────────────────────────────────────────────────┘
```

### A.2 Protocol Flow Diagram

```
Client 1    Client 2    Client N     Server
   │           │           │            │
   ├───────────┴───────────┴───────────►│ Round 1: Key Exchange
   │  Kyber PK, Capabilities            │
   │◄──────────────────────────────────┤│
   │  Kyber CT, Participant List        │
   │                                     │
   ├───────────────────────────────────►│ Round 2: Seed Sharing
   │  Encrypted Shamir Shares           │
   │◄──────────────────────────────────┤│
   │  Relayed Shares                    │
   │                                     │
   │  [Local Training]                  │
   │                                     │
   ├───────────────────────────────────►│ Round 3: Model Upload
   │  y_i = x_i + r_i, Signature        │
   │                                     │
   │                    [Aggregation]   │
   │                    z = Σx_i        │
   │◄──────────────────────────────────┤│
   │  Updated Global Model              │
   │                                     │
```

### A.3 Module Dependency Graph

```
qrfl/
├── crypto/          [No dependencies]
│   ├── kyber.py
│   ├── dilithium.py
│   └── shprg.py
│
├── protocol/        [Depends: crypto]
│   ├── client.py
│   ├── server.py
│   └── messages.py
│
├── security/        [Depends: protocol, crypto]
│   ├── byzantine.py
│   └── reputation.py
│
├── adaptive/        [Depends: protocol]
│   ├── profiler.py
│   └── variants.py
│
├── integrations/    [Depends: protocol]
│   ├── pytorch.py
│   └── tensorflow.py
│
└── cli/             [Depends: ALL]
    └── main.py
```

---

## Appendix B: Budget Breakdown (Detailed)

### B.1 Hardware Costs

| Item | Model | Qty | Unit Price | Total | Vendor |
|------|-------|-----|------------|-------|--------|
| Raspberry Pi 4B | 4GB RAM | 3 | $55 | $165 | Adafruit/Amazon |
| MicroSD Card | SanDisk 64GB | 3 | $10 | $30 | Amazon |
| USB-C Power Supply | 5V 3A | 3 | $8 | $24 | Amazon |
| Ethernet Cables | Cat6, 3ft | 3 | $5 | $15 | Amazon (optional) |
| **Subtotal** | | | | **$234** | |

### B.2 Cloud Costs (Optional, 2 months)

| Service | Instance Type | Hours/Month | Rate | Total |
|---------|---------------|-------------|------|-------|
| AWS EC2 | t3.medium | 100 | $0.0416/hr | $4.16 |
| AWS S3 | 10 GB | - | $0.023/GB | $0.23 |
| GitHub Actions | Private repos | - | Free tier | $0 |
| **Subtotal/Month** | | | | **$4.39** |
| **2 Months** | | | | **$8.78** |

### B.3 Total Investment

- Required (Hardware): **$234**
- Optional (Cloud): **$9**
- **Grand Total: $243**

Extremely cost-effective for PhD-level research with potential high-impact publication.

---

## Appendix C: Risk Register

| Risk ID | Description | Probability | Impact | Mitigation | Owner |
|---------|-------------|-------------|--------|------------|-------|
| R1 | RPi performance insufficient | Medium | High | Use Kyber512, ARM optimizations | Developer |
| R2 | Timeline overrun | Medium | Medium | 2-week buffer, MVP approach | Project Manager |
| R3 | Communication overhead not reduced | Low | Medium | Multiple optimization strategies | Developer |
| R4 | Byzantine detection <95% | Low | Medium | Threshold tuning, two-tier approach | Researcher |
| R5 | Hardware supply issues | Low | Low | Use alternative SBCs | Procurement |
| R6 | Paper rejection | Medium | Low | Incorporate feedback, resubmit | Researcher |
| R7 | Open-source adoption low | Medium | Low | Marketing, tutorials, community | Community Manager |

---

**This proposal is ready for review and approval. Implementation can begin immediately upon resource allocation.**

---

**Contact:**
[Your Name]  
[Email]  
[GitHub: github.com/yourusername]  
[Website: yourwebsite.com]
