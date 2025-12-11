# What is Federated Learning?

## Simple Analogy

Imagine you're a teacher trying to understand how students across 100 different schools learn best, but **privacy laws prevent you from seeing individual student data**.

**Traditional ML Approach (Centralized):**
- Collect all student records in one database
- Train one model on all the data
- ❌ Privacy violated, data exposed

**Federated Learning Approach (Decentralized):**
- Each school trains a local model on their own data
- Schools only share model improvements (weights/gradients), not raw data
- Central server aggregates these improvements
- ✅ Privacy preserved, no raw data leaves schools

## Technical Definition

Federated Learning (FL) is a **distributed machine learning paradigm** where:
1. Multiple clients (devices, organizations, edge nodes) collaboratively train a shared global model
2. Training data remains **decentralized** and never leaves the client devices
3. Only model updates (parameters, gradients) are communicated to a central server
4. The server aggregates updates to improve the global model

## Key Components

### 1. Clients (Participants)
- Hospitals, mobile devices, organizations
- Hold private local datasets
- Perform local training

### 2. Central Server (Aggregator)
- Coordinates the training process
- Aggregates model updates
- Distributes the global model

### 3. Communication Protocol
- Secure channels for update transmission
- Aggregation algorithms (e.g., FedAvg, FedProx)

## The Federated Learning Process

```
Round 1:
┌─────────────────────────────────────────────────┐
│ Central Server: Initialize Global Model M₀      │
└─────────────────────────────────────────────────┘
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
    Client 1    Client 2    Client 3
    (Hospital)  (Phone)     (Bank)
        │           │           │
    Train on    Train on    Train on
    local data  local data  local data
        │           │           │
    Compute     Compute     Compute
    ΔW₁         ΔW₂         ΔW₃
        │           │           │
        └───────────┼───────────┘
                    ↓
┌─────────────────────────────────────────────────┐
│ Server: Aggregate updates                       │
│ M₁ = M₀ + (ΔW₁ + ΔW₂ + ΔW₃) / 3               │
└─────────────────────────────────────────────────┘
                    │
                Repeat...
```

## Real-World Examples

### 1. Google Keyboard (Gboard)
- Millions of phones improve autocorrect predictions
- No typing data sent to Google servers
- Only model updates shared

### 2. Healthcare
- Multiple hospitals collaboratively train disease detection models
- Patient records never leave hospital premises
- HIPAA compliance maintained

### 3. Financial Services
- Banks detect fraud patterns across institutions
- Sensitive transaction data stays local
- Collective intelligence without data sharing

## Why Federated Learning Matters

### Privacy Preservation
- Sensitive data never leaves source
- Compliance with GDPR, HIPAA, CCPA
- User trust and data sovereignty

### Bandwidth Efficiency
- Don't need to transfer massive datasets
- Only lightweight model updates transmitted
- Suitable for edge computing

### Data Diversity
- Access heterogeneous data across silos
- Better model generalization
- Overcome data monopolies

## Basic Algorithm: Federated Averaging (FedAvg)

```python
# Pseudocode for FedAvg

# Server side
Initialize global model w⁰
for each round t = 1, 2, ..., T:
    Select subset of K clients randomly
    Send current global model wᵗ to selected clients
    
    # Client side (in parallel)
    for each client k in selected K:
        wᵏᵗ⁺¹ = LocalUpdate(wᵗ, local_data_k)
        Send wᵏᵗ⁺¹ back to server
    
    # Server aggregation
    wᵗ⁺¹ = Σ (nₖ/n) * wᵏᵗ⁺¹  # Weighted average
    
    where:
    nₖ = number of samples at client k
    n = total samples across all clients
```

## Mathematical Formulation

**Objective:** Minimize global loss function

$$
\min_{w} F(w) = \sum_{k=1}^{K} \frac{n_k}{n} F_k(w)
$$

Where:
- $F(w)$ = global objective function
- $F_k(w)$ = local objective at client $k$
- $n_k$ = number of samples at client $k$
- $K$ = total number of clients
- $w$ = model parameters

## Types of Federated Learning

### 1. Horizontal Federated Learning
- Clients have **same features**, different samples
- Example: Multiple hospitals with similar patient records
- Most common type

### 2. Vertical Federated Learning
- Clients have **different features**, overlapping samples
- Example: Bank + E-commerce company (same users, different data)

### 3. Federated Transfer Learning
- Clients have **different features AND different samples**
- Uses transfer learning techniques

## Key Challenges (Brief - detailed in separate file)

1. **Communication Efficiency** - High communication costs
2. **Statistical Heterogeneity** - Non-IID data distribution
3. **System Heterogeneity** - Different device capabilities
4. **Privacy Attacks** - Inference attacks on model updates
5. **Security** - Malicious clients, poisoning attacks

## The Security Gap: Why Post-Quantum Cryptography?

Current federated learning uses traditional cryptography (RSA, ECC) to secure:
- Model updates transmission
- Secure aggregation protocols
- Client authentication

**The Problem:** 
Quantum computers (Shor's algorithm) can break these cryptographic schemes, exposing:
- Model parameters (intellectual property)
- Gradient information (can leak training data)
- Aggregation keys (compromise entire system)

**The Solution:**
Post-quantum cryptography provides quantum-resistant security for federated learning systems.

---

## Further Reading

- McMahan et al. (2017) - "Communication-Efficient Learning of Deep Networks from Decentralized Data"
- Kairouz et al. (2021) - "Advances and Open Problems in Federated Learning"
- Yang et al. (2019) - "Federated Machine Learning: Concept and Applications"
