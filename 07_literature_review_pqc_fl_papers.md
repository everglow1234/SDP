# Literature Review: Post-Quantum Cryptography in Federated Learning

*Comprehensive analysis of 7 key papers (2022-2025) exploring PQC integration with FL systems*

---

## Overview

This document analyzes seven research papers that address the intersection of post-quantum cryptography (PQC) and federated learning (FL). The papers span 2022-2025 and represent different approaches to making FL systems quantum-resistant.

**Timeline of Publications:**
- **2022**: LaF (Xu et al.), Yang et al. Post-Quantum Secure Aggregation
- **2024**: Quantum FL with FHE
- **2025**: Performance testing (Paper #1), PQSF, V2G Authentication, PQBFL

---

## Paper #1: Performance Testing of PQC Algorithms in FL (2025)

### Focus
Empirical evaluation of NIST-standardized PQC algorithms (Dilithium, Falcon, SPHINCS+) in federated learning environments.

### Key Contributions
- **Hardware validation**: Testing on H100 GPU (high-end server environment)
- **Algorithm comparison**: Performance metrics for three signature schemes
- **Benchmark establishment**: Baseline overhead measurements for PQC in FL

### Technical Approach
- Direct replacement of classical signatures with PQC alternatives
- Performance testing: latency, throughput, computational overhead
- Focus on authentication and model signing

### Limitations
- **High-end hardware only**: No edge device validation (Raspberry Pi, IoT)
- **Component-level approach**: Testing algorithms individually, not system-level integration
- **No Byzantine resilience**: Semi-honest adversary model
- **Single-use focus**: Doesn't address multi-round key management optimization

### Gap for Our Work
Shows PQC is computationally feasible but doesn't address:
- Edge device constraints
- Multi-round protocol optimization
- Byzantine attack scenarios
- Heterogeneous device handling

---

## Paper #2: PQSF - Lattice-Based Secret Sharing (2025)

### Focus
Post-quantum secret sharing framework for secure aggregation using lattice-based cryptography.

### Key Contributions
- **Multi-use secret sharing**: Efficient schemes for multiple aggregation rounds
- **20% improvement claim**: Better performance than prior lattice approaches
- **Comparison schemes**: Shamir, Pilaram, Improved-Pilaram variants

### Technical Approach
- Lattice-based secret sharing (M-LWE hardness)
- Additively homomorphic properties for aggregation
- Multi-round key reuse strategies

### Limitations
- **Desktop simulation**: No real-world edge deployment
- **No Byzantine detection**: Assumes semi-honest participants
- **Communication overhead**: Quadratic scaling with participants (acknowledged but not solved)
- **Complexity**: Multiple secret sharing variants increase implementation difficulty

### Gap for Our Work
Provides cryptographic primitives but lacks:
- System-level protocol design
- Byzantine fault tolerance
- Edge device validation
- Simplicity for practical deployment

---

## Paper #3: V2G Authentication with PQC+FL (2025)

### Focus
Domain-specific application: Vehicle-to-Grid authentication using hierarchical FL with PQC.

### Key Contributions
- **Hierarchical FL**: Multi-tier architecture for V2G networks
- **PQC integration**: Quantum-resistant authentication for vehicles
- **Blockchain + FL**: Reputation management and authentication logging

### Technical Approach
- Hierarchical aggregation (edge → fog → cloud)
- PQC-based vehicle authentication
- Consortium blockchain with reputation scoring
- Byzantine detection through FL-based anomaly detection

### Limitations
- **Domain-specific**: Tailored to V2G, not general-purpose FL
- **High complexity**: Blockchain + hierarchical FL + PQC increases overhead
- **Limited scalability**: V2G-specific assumptions (fewer clients, longer training rounds)
- **No edge validation**: Simulated environment

### Gap for Our Work
Shows Byzantine detection is possible but approach is:
- Too complex for general FL
- Not validated on edge hardware
- Domain-locked (V2G-specific design decisions)

**Key Insight**: Byzantine detection via FL-based methods (not cryptographic verification) is interesting but computationally expensive.

---

## Paper #4: PQBFL - Ratcheting Mechanisms (2025)

### Focus
Post-quantum Byzantine-resilient federated learning with advanced key management.

### Key Contributions
- **Dual ratcheting**: Symmetric + asymmetric key derivation for multi-round security
- **Key management**: Signal-protocol-inspired approach for FL
- **Gas cost analysis**: 305-515 gas units per round on blockchain
- **Multi-round optimization**: Avoids key exchange every round

### Technical Approach
- **Symmetric ratcheting**: KDF chains for forward secrecy
- **Asymmetric ratcheting**: Periodic Kyber key updates
- **Blockchain integration**: HotStuff consensus for reputation/authentication
- **PUF + Zero-knowledge proofs**: Hardware-level security

### Limitations
- **High complexity**: Dual ratcheting + blockchain + PUF + ZKP = steep implementation curve
- **Gas costs**: Blockchain overhead (305-515 units/round) limits scalability
- **No edge validation**: Not tested on Raspberry Pi or constrained devices
- **Over-engineered**: Multiple security layers may exceed practical needs

### Gap for Our Work
**Closest to our vision** but missing:
- Edge device feasibility (Raspberry Pi validation)
- Simplicity (can we achieve Byzantine resistance with less complexity?)
- Communication efficiency (blockchain overhead)
- Heterogeneity handling

**Positioning**: "We achieve Byzantine resistance with simpler protocol design while validating on edge devices—PQBFL's dual ratcheting is powerful but too complex for resource-constrained FL deployments."

---

## Paper #5: Quantum Computing + FHE for FL (2024)

### Focus
Hybrid quantum-classical FL using quantum computing for local training and FHE for privacy.

### Key Contributions
- **Quantum local training**: Quantum circuits for model updates
- **FHE for aggregation**: Fully homomorphic encryption preserves privacy
- **Hybrid architecture**: Classical server + quantum clients

### Technical Approach
- Variational quantum circuits (VQC) for local model training
- FHE (CKKS/BFV) for encrypted aggregation
- Quantum advantage in specific learning tasks

### Limitations
- **Orthogonal focus**: Privacy during training (not protocol-level security)
- **Hardware unavailable**: Requires quantum computers (not accessible for most FL scenarios)
- **Not post-quantum secure**: FHE doesn't address quantum attacks on communication protocols
- **Research-stage**: Far from practical deployment

### Gap for Our Work
**Different problem domain**: This addresses training privacy, we address protocol security.

**Not a competitor**: Our work focuses on quantum-resistant authentication/aggregation protocols, not quantum computing for training.

---

## Paper #6: LaF - Lattice-Based Federated Learning (2022)

### Focus
**Foundational work**: First comprehensive lattice-based secure aggregation for FL.

### Key Contributions
- **Multi-use lattice secret sharing**: Efficient reuse across rounds
- **Communication optimization**: Improved topology over SecAgg
- **Post-quantum security**: LWE-based hardness assumptions
- **Dropout resilience**: Threshold secret sharing handles participant failures

### Technical Approach
- Lattice-based secret sharing (extends Shamir to LWE)
- Double masking: Pairwise shared secrets
- Graph topology optimization (k-regular graphs)
- Threshold reconstruction for dropout tolerance

### Limitations
- **Double masking overhead**: Computation scales with dropped participants
- **Communication complexity**: Still quadratic in worst case
- **No Byzantine resilience**: Semi-honest adversary model only
- **Desktop simulation**: Not validated on edge devices

### Historical Significance
**Baseline for all subsequent work**: Every paper after 2022 references LaF as the foundation.

### Gap for Our Work
**We build on LaF's foundation** by adding:
- Byzantine fault tolerance
- Single masking (simpler than double masking)
- Edge device validation
- Heterogeneity handling

---

## Paper #7: Yang et al. - Post-Quantum Secure Aggregation (2022)

### Focus
**Efficient 3-round PQC secure aggregation** using single masking and Kyber KEM.

### Key Contributions
- **Single masking**: SHPRG (Seed Homomorphic PRG) based on Learning with Rounding
- **3-round protocol**: Reduced from 4 rounds in SecAgg
- **Kyber KEM**: Post-quantum secure channels
- **Linear dropout scaling**: Overhead independent of dropped participants (vs quadratic in SecAgg)
- **Sub-second aggregation**: 100 participants, 50K model size on Intel i9

### Technical Approach
1. **Round 1**: Kyber KEM key exchange between participants
2. **Round 2**: Shamir secret sharing of SHPRG seeds
3. **Round 3**: Encrypted model upload + mask recovery

**Key Innovation**: Single mask `r_i = SHPRG(s_i)` where homomorphic property allows `Σr_i = SHPRG(Σs_i)`

### Limitations (Acknowledged by Authors)
- **High communication overhead**: Quadratic scaling (authors cite as "open question")
- **Semi-honest only**: No Byzantine resilience
- **High-end hardware**: Intel i9-10920X testing only
- **Basic heterogeneity**: All participants assumed similar capabilities

### Why This is Key Comparison Baseline
1. **Same PQC primitives**: Kyber KEM (we'll likely use same)
2. **Single masking**: Good foundation to build upon
3. **Explicit open question**: Authors left communication optimization unsolved
4. **Performance benchmark**: Our Pi results vs their i9 results demonstrate edge feasibility

### Gap for Our Work
**Direct improvements**:
- Extend semi-honest → Byzantine-resistant
- Optimize communication overhead (their acknowledged weakness)
- Validate on edge devices (Pi vs i9)
- Add heterogeneity handling

**Positioning Statement**:
*"While Yang et al. (2022) achieved efficient single-masking secure aggregation, their approach remains vulnerable to Byzantine attacks and suffers from quadratic communication overhead. We extend their foundation with Byzantine detection mechanisms, communication-optimized protocols, and real-world edge device validation."*

---

## Comparative Analysis: Evolution of the Field

### Chronological Development

**2022 - Foundation Year**:
- **LaF**: Established lattice-based multi-use secret sharing (double masking)
- **Yang et al.**: Simplified to single masking with SHPRG (better dropout handling)

**2024 - Diversification**:
- **Quantum FL**: Explored orthogonal direction (quantum training + FHE)

**2025 - Specialization & Enhancement**:
- **Paper #1**: Empirical validation of NIST standards
- **PQSF**: Improved secret sharing variants
- **V2G Paper**: Domain-specific application with Byzantine detection
- **PQBFL**: Advanced key management with dual ratcheting

### Technical Approaches Comparison

| Paper | Masking | Rounds | Byzantine | Edge Validated | Key Innovation |
|-------|---------|--------|-----------|----------------|----------------|
| **LaF (2022)** | Double | 4 | ❌ | ❌ | First lattice-based FL |
| **Yang (2022)** | Single | 3 | ❌ | ❌ | SHPRG single masking |
| **Paper #1 (2025)** | N/A | 1 | ❌ | ❌ | NIST algorithm testing |
| **PQSF (2025)** | Multi-use | 2-3 | ❌ | ❌ | 20% improvement on LaF |
| **V2G (2025)** | Various | 3+ | ✅ | ❌ | FL-based Byzantine detection |
| **PQBFL (2025)** | Ratcheting | 2 | ✅ | ❌ | Dual ratcheting + blockchain |
| **Quantum FL (2024)** | FHE | 2 | ❌ | ❌ | Quantum local training |
| **OUR WORK** | **TBD** | **2-3** | **✅** | **✅** | **Complete framework** |

### Gap Analysis Summary

**What's Missing Across ALL Papers**:

1. **Edge Device Validation** ❌
   - All papers test on desktop/server (H100 GPU, Intel i9)
   - No Raspberry Pi or IoT device experiments
   - **Our contribution**: First edge-validated PQC+FL framework

2. **Byzantine + PQC Together** ⚠️
   - V2G & PQBFL attempt but with limitations:
     - V2G: Domain-specific, FL-based detection (not cryptographic)
     - PQBFL: Over-engineered, no edge validation
   - **Our contribution**: Cryptographic Byzantine detection + edge feasibility

3. **Communication Efficiency** ⚠️
   - Yang et al. explicitly state as open question
   - PQSF claims 20% improvement but still quadratic
   - PQBFL adds blockchain overhead (gas costs)
   - **Our contribution**: Optimized multi-round protocol design

4. **Heterogeneity Handling** ❌
   - No paper addresses diverse device capabilities
   - All assume homogeneous participants
   - **Our contribution**: Adaptive protocols for heterogeneous FL

5. **Simplicity for Deployment** ⚠️
   - PQBFL: Too complex (ratcheting + blockchain + PUF + ZKP)
   - PQSF: Multiple secret sharing variants
   - **Our contribution**: Balance between security and practical implementability

---

## Positioning Our Framework

### Novelty Statement

**"First comprehensive Byzantine-resistant post-quantum federated learning framework validated on edge devices with optimized communication protocols."**

### Unique Contributions

1. **System-Level Design** (vs Component Replacement)
   - Not just "add PQC to existing FL"
   - Protocol redesign from ground up for quantum resistance

2. **Byzantine Resilience + PQC** (vs Semi-Honest Only)
   - LaF, Yang, PQSF: Semi-honest only
   - V2G: Domain-specific Byzantine detection
   - PQBFL: Too complex
   - **Ours**: Practical Byzantine resistance with PQC

3. **Edge Validation** (vs Simulation/High-End)
   - ALL prior work: Desktop or high-end server testing
   - **Ours**: Raspberry Pi 4 experimental validation

4. **Communication Optimization** (vs Acknowledged Weakness)
   - Yang et al.: "Open question"
   - PQSF: Still quadratic
   - **Ours**: Multi-round key management optimization

5. **Heterogeneity** (vs Homogeneous Assumptions)
   - No prior work addresses device diversity
   - **Ours**: Adaptive protocol selection based on device capabilities

### Research Gaps We Fill

| Gap | Prior Work | Our Solution |
|-----|-----------|--------------|
| **Edge feasibility** | No validation | Raspberry Pi experiments (3 devices) |
| **Byzantine + PQC** | Too complex (PQBFL) or domain-specific (V2G) | Balanced approach with cryptographic verification |
| **Communication** | Quadratic overhead (Yang et al.) | FL-specific multi-round optimization |
| **Heterogeneity** | Not addressed | Device profiling + adaptive protocols |
| **Simplicity** | Over-engineered (PQBFL: 5+ layers) | Streamlined 3-layer protocol |

---

## Related Work Section Structure (For Paper)

### Proposed Organization

**1. Foundational Secure Aggregation**
   - SecAgg (Bonawitz 2017) - baseline MPC approach
   - LaF (Xu 2022) - first lattice-based extension
   - Yang et al. (2022) - single masking innovation

**2. Post-Quantum Cryptography in FL**
   - Paper #1 (2025) - NIST algorithm performance testing
   - PQSF (2025) - improved secret sharing
   - Quantum FL (2024) - orthogonal approach (training privacy)

**3. Byzantine-Resilient Approaches**
   - V2G (2025) - FL-based detection (domain-specific)
   - PQBFL (2025) - dual ratcheting (high complexity)

**4. Our Positioning**
   - "We synthesize Byzantine resilience + PQC efficiency + edge feasibility"
   - "First framework addressing ALL three simultaneously"

---

## Key Takeaways for Framework Design

### What to Adopt

1. **Kyber KEM** (Yang et al.)
   - Proven post-quantum secure
   - NIST standard (Paper #1 validation)
   - Reasonable overhead

2. **Single Masking** (Yang et al.)
   - Simpler than double masking (LaF)
   - Better dropout handling
   - Lower computation overhead

3. **Threshold Secret Sharing** (LaF, Yang, PQSF)
   - Dropout resilience
   - Information-theoretic security
   - Established technique

4. **Multi-Round Key Reuse** (PQSF, PQBFL)
   - Avoid key exchange every round
   - Significant communication savings
   - Forward secrecy considerations

### What to Improve

1. **Communication Overhead** (Yang et al.'s open question)
   - Current: Quadratic scaling
   - Target: Logarithmic or linear with optimizations

2. **Byzantine Detection** (V2G approach too complex)
   - Simpler than PQBFL's 5-layer approach
   - More general than V2G's domain-specific solution
   - Cryptographic verification + reputation scoring

3. **Edge Validation** (Nobody has done this)
   - Raspberry Pi 4 experiments
   - Demonstrate practical feasibility
   - Real-world latency/energy measurements

4. **Heterogeneity** (Unexplored territory)
   - Device profiling at setup
   - Adaptive protocol selection
   - Graceful degradation for weak devices

### What to Avoid

1. **Over-Engineering** (PQBFL's mistake)
   - Don't need: PUF + ZKP + blockchain + dual ratcheting + reputation
   - Need: Minimal sufficient security layers

2. **Domain Lock-In** (V2G's limitation)
   - Keep framework general-purpose
   - V2G-specific assumptions limit applicability

3. **Simulation-Only** (Most papers' weakness)
   - Must validate on real hardware
   - Raspberry Pi deployment essential

4. **High-End Hardware Assumptions** (Paper #1)
   - H100 GPU not representative of edge devices
   - Design for constraints, not abundance

---

## Next Steps: Framework Architecture Design

### Critical Decisions Based on Literature

1. **Cryptographic Primitives**
   - ✅ Kyber KEM (established choice)
   - ❓ Signature scheme: Dilithium vs Falcon?
   - ❓ PRG approach: SHPRG (Yang) vs alternatives?

2. **Masking Strategy**
   - ✅ Single masking (Yang approach)
   - ❓ Homomorphic properties: LWR vs LWE?
   - ❓ Seed management: Shamir threshold value?

3. **Byzantine Detection**
   - ❓ Cryptographic (signature verification)?
   - ❓ FL-based (gradient anomaly)?
   - ❓ Hybrid approach?

4. **Multi-Round Optimization**
   - ❓ Simple symmetric ratcheting (vs PQBFL's dual)?
   - ❓ Session-based key reuse?
   - ❓ Periodic refresh intervals?

5. **Heterogeneity Handling**
   - ❓ Device classes: Low/Medium/High capability?
   - ❓ Protocol variants for each class?
   - ❓ Dynamic adaptation based on runtime performance?

### Framework Layers (Tentative)

**Layer 1: Post-Quantum Secure Channel**
- Kyber KEM key exchange
- AES-GCM authenticated encryption

**Layer 2: Identity & Authentication**
- PQC digital signatures (Dilithium/Falcon)
- Participant enrollment

**Layer 3: Secure Aggregation Protocol**
- Single masking with SHPRG
- Shamir secret sharing (threshold = 2/3 participants)
- 3-round communication

**Layer 4: Byzantine Detection**
- Signature verification (cryptographic)
- Gradient anomaly detection (statistical)
- Reputation scoring (optional, simpler than blockchain)

**Layer 5: Multi-Round Key Management**
- Symmetric ratcheting for session keys
- Periodic Kyber key refresh (every 10 rounds?)

**Layer 6: Heterogeneity Adaptation**
- Device profiling at setup
- Protocol variant selection
- Dynamic timeout adjustments

---

## Conclusion

The analysis of these 7 papers reveals a rapidly evolving field with significant progress in PQC+FL integration, but critical gaps remain:

**Strengths of Existing Work:**
- Solid cryptographic foundations (LaF, Yang et al.)
- Proven PQC algorithms (Paper #1)
- Initial Byzantine approaches (V2G, PQBFL)

**Persistent Gaps (Our Opportunity):**
- No edge device validation
- Communication overhead unsolved
- Byzantine+PQC not practically combined
- Heterogeneity ignored

**Our Framework Positioning:**
"The first complete, practical, and validated post-quantum Byzantine-resistant federated learning framework designed for heterogeneous edge deployments."

**Timeline Advantage:**
December 2025 is perfect timing—we have 3 years of foundational work (2022) + recent innovations (2025) to build upon, while the field hasn't yet solved the integration challenge.

---

**Ready to design the framework architecture?** We now have clear baselines (Yang et al., PQBF), proven primitives (Kyber, SHPRG), and identified gaps to fill.
