# Post-Quantum Cryptography (PQC)

## The Quantum Threat

### Simple Analogy
Imagine you have a lock that takes a classical computer **1 million years** to pick. A quantum computer could pick it in **minutes**. Post-quantum cryptography is building locks that even quantum computers can't pick quickly.

## What is Post-Quantum Cryptography?

**Definition:** Cryptographic algorithms that are secure against attacks by both classical and quantum computers.

### Why Do We Need It?

**Shor's Algorithm (1994)** - Quantum algorithm that can:
- Factor large integers in polynomial time
- Break RSA encryption
- Break Elliptic Curve Cryptography (ECC)
- Break Diffie-Hellman key exchange

**Timeline:**
- RSA-2048 (current standard): Classical computer needs ~300 trillion years
- Quantum computer with sufficient qubits: Could break in hours/days
- "Harvest now, decrypt later" attacks are already happening

## Post-Quantum Cryptographic Schemes

### 1. Lattice-Based Cryptography ⭐ (Most Promising)

**Core Concept:** Based on hard problems in high-dimensional lattices

**Hard Problem:** Learning With Errors (LWE)
- Given: $(A, b = As + e)$ where $e$ is small error
- Find: Secret vector $s$
- Quantum computers can't solve efficiently

**Advantages:**
- ✅ Strong security proofs
- ✅ Efficient implementation
- ✅ Supports homomorphic encryption
- ✅ Good performance

**Examples:**
- CRYSTALS-Kyber (Key Encapsulation)
- CRYSTALS-Dilithium (Digital Signatures)
- NTRU
- FrodoKEM

**Use in Federated Learning:**
- Secure aggregation protocols
- Encrypted model updates
- Homomorphic operations on gradients

### 2. Hash-Based Cryptography

**Core Concept:** Based on security of cryptographic hash functions

**Hard Problem:** Finding hash collisions (still hard for quantum computers)

**Advantages:**
- ✅ Well-understood security
- ✅ Minimal security assumptions
- ✅ Fast signing

**Disadvantages:**
- ❌ Large signature sizes
- ❌ Stateful (complex key management)

**Examples:**
- XMSS (eXtended Merkle Signature Scheme)
- SPHINCS+

**Use in Federated Learning:**
- Client authentication
- Model integrity verification

### 3. Code-Based Cryptography

**Core Concept:** Based on error-correcting codes

**Hard Problem:** Decoding random linear codes

**Advantages:**
- ✅ Fast encryption/decryption
- ✅ Oldest PQC approach (40+ years)

**Disadvantages:**
- ❌ Very large key sizes (hundreds of KB to MB)

**Examples:**
- Classic McEliece
- BIKE (Bit Flipping Key Encapsulation)

**Use in Federated Learning:**
- Limited due to large keys
- Possibly for long-term encryption

### 4. Multivariate Polynomial Cryptography

**Core Concept:** Solving systems of multivariate polynomial equations

**Hard Problem:** MQ problem (NP-hard)

**Advantages:**
- ✅ Very fast signature verification
- ✅ Short signatures

**Disadvantages:**
- ❌ Large public keys
- ❌ Some schemes broken in the past

**Examples:**
- Rainbow (recently broken - 2022)
- GeMSS
- UOV

**Use in Federated Learning:**
- Fast signature verification for model updates

### 5. Isogeny-Based Cryptography

**Core Concept:** Based on isogenies between elliptic curves

**Hard Problem:** Finding isogenies between supersingular curves

**Advantages:**
- ✅ Smallest key sizes among PQC

**Disadvantages:**
- ❌ Slower performance
- ❌ SIDH was broken (2022)

**Examples:**
- CSIDH
- SQISign

**Use in Federated Learning:**
- Bandwidth-constrained scenarios

## NIST Post-Quantum Cryptography Standardization

### Selected Algorithms (2022-2024)

**For Encryption/Key Establishment:**
1. **CRYSTALS-Kyber** ⭐ (Primary)
   - Lattice-based KEM
   - Key size: 800-1,568 bytes
   - Fast and secure

**For Digital Signatures:**
1. **CRYSTALS-Dilithium** ⭐ (Primary)
   - Lattice-based signatures
   - Signature size: 2,420-4,595 bytes

2. **Falcon**
   - Lattice-based (NTRU)
   - Smaller signatures than Dilithium
   - More complex implementation

3. **SPHINCS+**
   - Hash-based
   - Conservative security choice
   - Large signatures (8-50 KB)

### Round 4 Candidates (Additional Signatures)
- BIKE, HQC, Classic McEliece (alternative key encapsulation)
- SQISign, MAYO, CROSS (additional signatures)

## Comparison with Classical Cryptography

| Property | RSA-2048 | ECC-256 | Kyber-768 | Dilithium-3 |
|----------|----------|---------|-----------|-------------|
| **Public Key Size** | 256 bytes | 32 bytes | 1,184 bytes | 1,952 bytes |
| **Secret Key Size** | 256 bytes | 32 bytes | 2,400 bytes | 4,000 bytes |
| **Signature/Ciphertext** | 256 bytes | 64 bytes | 1,088 bytes | 3,293 bytes |
| **Quantum Secure** | ❌ | ❌ | ✅ | ✅ |
| **Performance** | Slow | Fast | Fast | Medium |

## Key Challenges of PQC

### 1. Larger Key and Signature Sizes
- 10-100x larger than classical cryptography
- Impact on bandwidth in federated learning

### 2. Performance Overhead
- More computation required
- Battery drain on mobile devices

### 3. Implementation Complexity
- Side-channel attack vulnerabilities
- Constant-time implementations needed

### 4. Standardization and Migration
- Transitioning existing systems
- Hybrid approaches (classical + PQC)

## Cryptographic Primitives Needed in Federated Learning

### 1. Key Encapsulation Mechanism (KEM)
- **Purpose:** Establish shared secret keys
- **PQC Solution:** Kyber
- **Use Case:** Secure channel establishment between clients and server

### 2. Digital Signatures
- **Purpose:** Authentication and integrity
- **PQC Solution:** Dilithium, Falcon
- **Use Case:** Sign model updates, verify server identity

### 3. Public Key Encryption (PKE)
- **Purpose:** Encrypt data to specific recipients
- **PQC Solution:** Kyber (as PKE), ElGamal-like lattice schemes
- **Use Case:** Secure aggregation protocols

### 4. Homomorphic Encryption (Advanced)
- **Purpose:** Compute on encrypted data
- **PQC Solution:** BFV, CKKS (lattice-based)
- **Use Case:** Private aggregation of model gradients

## Example: Kyber in Action

```python
# Simplified Kyber Key Encapsulation

# Key Generation
(public_key, secret_key) = Kyber.KeyGen()
# public_key: 1184 bytes
# secret_key: 2400 bytes

# Encapsulation (sender side)
(ciphertext, shared_secret_sender) = Kyber.Encaps(public_key)
# ciphertext: 1088 bytes
# shared_secret: 32 bytes

# Decapsulation (receiver side)
shared_secret_receiver = Kyber.Decaps(ciphertext, secret_key)

# Now both parties share the same secret
assert shared_secret_sender == shared_secret_receiver

# Use shared secret for symmetric encryption (AES)
encrypted_model_update = AES_Encrypt(shared_secret, model_update)
```

## Security Levels

NIST defines 5 security levels based on key search complexity:

| Level | Classical Security | Quantum Security | Example |
|-------|-------------------|------------------|---------|
| 1 | AES-128 | Grover search on AES-128 | Kyber-512 |
| 3 | AES-192 | Grover search on AES-192 | Kyber-768 ⭐ |
| 5 | AES-256 | Grover search on AES-256 | Kyber-1024 |

**Recommendation for FL:** Level 3 (balance security and performance)

## Quantum-Resistant Properties

### What Makes These Algorithms Secure?

1. **Lattice Problems:** Quantum computers don't have efficient algorithms
2. **Hash Functions:** Still secure (Grover's algorithm only quadratic speedup)
3. **Mathematical Structure:** No hidden algebraic structure to exploit

### Known Quantum Attacks and Defenses

| Attack | Impact on Classical | Impact on PQC |
|--------|-------------------|---------------|
| Shor's Algorithm | ❌ Breaks RSA/ECC | ✅ No impact |
| Grover's Algorithm | ⚠️ Halves key strength | ⚠️ Halves key strength |
| Quantum Search | ⚠️ Speeds up attacks | ⚠️ Parameters adjusted |

## Hybrid Approaches

**Concept:** Use both classical and post-quantum cryptography

```
Hybrid_Key = KDF(Classical_Key || PQC_Key)
```

**Advantages:**
- Security if either algorithm is broken
- Smooth migration path
- Current best practice

**Example:**
- X25519 (ECC) + Kyber-768
- Used in Chrome, Signal, Cloudflare

---

## Resources

- NIST PQC Project: https://csrc.nist.gov/projects/post-quantum-cryptography
- PQShield: https://pqshield.com/
- Open Quantum Safe: https://openquantumsafe.org/
