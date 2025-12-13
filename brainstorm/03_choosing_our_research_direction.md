# Choosing Our Research Direction

Hey Vaishu and Yasaswini! 👋

Ok, so by now you've read through everything:
- ✅ **01.md:** What FL is, how it works, and ALL 12 challenges
- ✅ **02.md:** Why we need PQC (quantum threat is real!)
- ✅ **02a.md:** How quantum breaks crypto + what PQC algorithms exist

Now comes the hard part: **What do WE actually work on?** 🤔

Let me lay out the situation clearly...

---

## 🌪️ The Brutal Truth: NOTHING is Perfect

Here's what we learned:

### **Federated Learning is NOT Perfect** ❌

From 01.md, we saw **12 major challenges**:
1. Communication efficiency (3TB → 180KB but still huge for FL)
2. Non-IID data (everyone's data is different)
3. System heterogeneity (different devices, different speeds)
4. Privacy leakage (gradients leak information)
5. **Byzantine attacks** (malicious clients poisoning the model) ⚠️
6. **Security vs privacy trade-off** ⚠️
7. Convergence issues (model doesn't converge well)
8. Fairness (some clients benefit less)
9. Client dropout (devices go offline)
10. Incentive mechanisms (why should clients participate?)
11. Scalability (1000s of clients is hard)
12. Debugging (can't see what's happening inside clients)

**Reality:** FL solves the "data leaves the building" problem, but creates 12 new problems! 😅

---

### **Post-Quantum Cryptography is NOT Perfect** ❌

From 02a.md, we saw **8 major challenges**:
1. **Large keys** (37× bigger than ECC) → Bandwidth explosion
2. **Slower operations** (2-14× slower) → Battery drain
3. **Immaturity** (only 8 years old) → Unknown attacks possible
4. **Implementation complexity** → Side-channel vulnerabilities
5. **Slow deployment** → Industry inertia
6. **Patents** → Legal uncertainty
7. **Timeline uncertainty** → When to migrate?
8. **PQC Secure Aggregation** → No good solution exists! ⚠️

**Reality:** PQC protects from quantum computers, but makes everything bigger, slower, and more complex! 😅

---

### **Byzantine Defenses are NOT Perfect** ❌

We haven't written a full document on this yet, but here's the reality:

**Byzantine-Robust Aggregation Methods:**
- Krum, Trimmed Mean, Median, Bulyan, FLTrust, etc.

**Problems:**
1. **Accuracy loss:** Robust aggregation reduces model accuracy (5-15% drop)
2. **Computational overhead:** Need to compute distances/outliers (expensive!)
3. **Only detects SOME attacks:** New attack types keep emerging
4. **Collusion attacks:** If multiple malicious clients coordinate, detection fails
5. **Non-IID data:** Hard to tell if a gradient is "malicious" or just "different data"
6. **No formal guarantees:** Most defenses are heuristic (no proof they work)

**Reality:** Byzantine defenses help, but don't SOLVE the problem completely! 😅

---

## ⚖️ The MAJOR Trade-offs We Face

Here's the brutal reality of our situation:

### **Trade-off 1: Security vs Privacy vs Communication Efficiency**

```
You can pick TWO, but not all THREE:

Option A: Security + Privacy
├── Use PQC encryption + Byzantine defense + Secure aggregation
├── ✅ Quantum-safe
├── ✅ Protects against attacks
├── ✅ Privacy preserved
└── ❌ MASSIVE communication overhead (100×)

Option B: Privacy + Efficiency
├── Use simple secure aggregation (no PQC, no Byzantine defense)
├── ✅ Privacy preserved
├── ✅ Low communication cost
└── ❌ Vulnerable to quantum attacks + Byzantine attacks

Option C: Security + Efficiency
├── No encryption, just Byzantine defense
├── ✅ Efficient
├── ✅ Protects from malicious clients
└── ❌ NO privacy (server sees all gradients)

We WANT all three, but it's HARD! 🤯
```

---

### **Trade-off 2: PQC Security vs Bandwidth**

From 02a.md Challenge 1:

```
Classical Crypto (ECC-256):
├── Public key: 32 bytes
├── Signature: 64 bytes
├── Fast, small, efficient ✅
└── Quantum breaks it in 1 hour ❌

Post-Quantum (Kyber-768 + Dilithium-3):
├── Public key: 1,184 + 1,952 = 3,136 bytes (98× larger!)
├── Signature: 3,293 bytes (51× larger!)
├── Quantum-safe ✅
└── Huge bandwidth cost ❌

For 100 clients × 50 rounds:
├── ECC: 5,000 × 96 bytes = 480 KB
└── PQC: 5,000 × 6,429 bytes = 31 MB (65× more!)

Can we have quantum-safety WITHOUT huge keys?
This is Challenge 8 from 02a.md!
```

---

### **Trade-off 3: Byzantine Defense vs Model Accuracy**

```
No Defense:
├── Accuracy: 95% (baseline)
├── Security: 0% (one malicious client destroys model)
└── ❌ Unusable in real world

Krum (Byzantine defense):
├── Accuracy: 88% (7% drop!)
├── Security: 80% (detects most attacks)
└── ⚠️ Trade-off accuracy for security

FLTrust (better defense):
├── Accuracy: 91% (4% drop)
├── Security: 90% (better detection)
├── But: Needs server-side data (breaks privacy premise!)
└── ⚠️ Trade-off privacy for accuracy

Can we have security WITHOUT accuracy loss?
No perfect solution exists!
```

---

### **Trade-off 4: Research Depth vs Timeline**

Remember from README.md, we have **4 months for SDP**.

```
Path A (2-3 weeks):
├── Study existing solutions
├── Implement and compare
├── Write report
└── ⚠️ No novelty, but guaranteed to finish

Path B (2-3 months):
├── Implement existing + propose small improvement
├── Run experiments
├── Write paper
└── ⚠️ Medium risk, medium novelty

Path C (6-12 months):
├── Novel algorithm/framework
├── Extensive experiments
├── Publish in conference
└── ⚠️ High risk, high reward (but we only have 4 months!)

We physically cannot do Path C!
Need to be realistic about timeline.
```

---

## 🎯 What Should WE Work On?

Ok, given all these trade-offs, here are our options:

---

### **Option 1: Byzantine-Robust FL with PQC** 🎯 (RECOMMENDED)

**The Problem:**
- Current FL uses classical crypto → Quantum breaks it
- Current Byzantine defenses don't use PQC → Vulnerable in quantum era
- Combining PQC + Byzantine defense → Huge overhead (100× communication)

**Our Goal:**
Design efficient Byzantine-robust FL that uses PQC without exploding communication cost.

**Specifically:**
- Replace classical crypto with PQC in existing Byzantine defense (like Krum)
- Optimize to reduce communication overhead (compression, sampling, etc.)
- Show it's practical (experiments with real datasets)

**Why This Makes Sense:**
✅ Combines FL Challenge 5 (Byzantine) + 02a Challenge 8 (PQC Secure Aggregation)
✅ Sir wants "something in cryptography" → PQC is perfect
✅ Clear problem statement
✅ Existing baselines to compare against
✅ Realistic for 4 months (Path B)
✅ Novelty: No existing work combines Byzantine + PQC efficiently

**Challenges:**
⚠️ Need to understand Byzantine defense algorithms deeply
⚠️ Need to implement PQC libraries (Kyber/Dilithium)
⚠️ Experiments might show it's still too slow
⚠️ May need multiple iterations to optimize

**Timeline (4 months):**
```
Month 1: Study Byzantine defenses + PQC implementation
Month 2: Design our approach + implement prototype
Month 3: Run experiments + optimize
Month 4: Write paper/report + prepare presentation
```

---

### **Option 2: Compact PQC for FL** (See 06.md)

**The Problem:**
- PQC keys are 37× larger than classical
- FL needs thousands of key exchanges
- Communication explodes → Impractical for mobile/IoT

**Our Goal:**
Design compact PQC specifically optimized for FL constraints.

**Specifically:**
- Explore structured lattices (NTRU, module-LWE)
- Use compression techniques (NTT domain, sparse representations)
- Trade-off security level for compactness (maybe 112-bit instead of 128-bit)
- Show it's practical for FL

**Why This Makes Sense:**
✅ Directly addresses 02a Challenge 1 (large keys)
✅ Pure cryptography (sir will love this)
✅ Clear metrics (key size, speed, security level)
✅ 06.md already has foundation

**Challenges:**
⚠️⚠️ VERY math-heavy (need strong crypto background)
⚠️⚠️ Security proofs required (are we weakening it too much?)
⚠️⚠️ Might be too hard for 4 months (Path C difficulty)
⚠️ Implementation bugs could break security

**Timeline (4 months):**
```
Month 1: Deep dive into lattice crypto math
Month 2: Design compact scheme + security analysis
Month 3: Implement + benchmark
Month 4: Security proofs + write paper
```

**Reality Check:** This is probably TOO hard for 4 months unless one of us is a crypto expert! 😅

---

### **Option 3: Byzantine Defense for Non-IID Data**

**The Problem:**
- Current Byzantine defenses assume IID data
- In real FL, data is Non-IID (Dirichlet α=0.5)
- Hard to tell if gradient is "malicious" or just "different data"
- False positives → Exclude legitimate clients!

**Our Goal:**
Design Byzantine defense that works well under Non-IID conditions.

**Specifically:**
- Study why existing defenses fail with Non-IID
- Design defense that considers data heterogeneity
- Use clustering or personalization techniques
- Show it improves accuracy + security

**Why This Makes Sense:**
✅ Combines FL Challenge 2 (Non-IID) + Challenge 5 (Byzantine)
✅ Very practical problem (real FL is always Non-IID)
✅ Good research direction

**Challenges:**
⚠️ NO cryptography component → Sir wants crypto!
⚠️ Might not satisfy project requirement
⚠️ Hard to define "ground truth" (when is a gradient malicious vs just different?)

**Timeline (4 months):**
```
Month 1: Study Byzantine defenses + Non-IID FL
Month 2: Design approach
Month 3: Implement + experiments
Month 4: Write paper
```

**Reality Check:** Good problem, but doesn't satisfy "cryptography requirement" from sir! ❌

---

### **Option 4: Privacy-Preserving Byzantine-Robust FL**

**The Problem:**
- Byzantine defenses need to see gradients (no privacy!)
- Secure aggregation provides privacy (but no Byzantine defense!)
- Trade-off between security and privacy
- Can we have BOTH?

**Our Goal:**
Design FL system that defends against Byzantine attacks while preserving privacy.

**Specifically:**
- Use homomorphic encryption or secure multi-party computation
- Detect Byzantine attacks in encrypted domain
- Show it's possible (even if slow)

**Why This Makes Sense:**
✅ Addresses FL Challenge 6 (security vs privacy trade-off)
✅ Fundamental research problem
✅ Very impactful if solved

**Challenges:**
⚠️⚠️⚠️ EXTREMELY HARD (unsolved research problem!)
⚠️⚠️⚠️ Might be impossible (detecting outliers in encrypted data is hard)
⚠️⚠️ Needs advanced crypto (homomorphic encryption, MPC)
⚠️⚠️ Definitely can't finish in 4 months

**Reality Check:** This is PhD-level difficulty! Way too hard for 4 months! ❌

---

## 🎲 My Recommendation: Option 1 (Byzantine + PQC)

Here's why I think **Option 1** is our best bet:

### ✅ **Advantages:**
1. **Fits sir's requirement:** Has strong cryptography component (PQC)
2. **Realistic scope:** Doable in 4 months (Path B)
3. **Clear problem:** Existing solutions + clear improvement direction
4. **Novelty:** Nobody has combined Byzantine + PQC efficiently yet
5. **Practical impact:** Quantum threat is real, we need this eventually
6. **Good baselines:** Can compare against classical Byzantine defenses
7. **Measurable metrics:** Communication cost, accuracy, attack detection rate

### ⚠️ **Risks:**
1. Implementation complexity (need to learn PQC libraries)
2. Might still be too slow (need creative optimization)
3. Experiments take time (need powerful machines)

### 📋 **Concrete Plan:**

**Week 1-2:** Study Byzantine defenses in depth
- Read papers: Krum, Trimmed Mean, Bulyan, FLTrust
- Understand how they work mathematically
- Implement classical versions

**Week 3-4:** Study PQC implementation
- Learn Kyber/Dilithium libraries (CRYSTALS)
- Understand key exchange protocols
- Replace classical crypto with PQC in simple FL

**Week 5-6:** Design our approach
- Combine Byzantine defense + PQC
- Identify bottlenecks (key size? signatures?)
- Brainstorm optimizations (compression, sampling, caching)

**Week 7-10:** Implementation
- Build prototype FL system
- Integrate Byzantine defense + PQC
- Test on small datasets

**Week 11-13:** Experiments
- Run on MNIST, CIFAR-10, FEMNIST
- Measure: accuracy, communication, attack detection
- Compare against baselines
- Optimize based on results

**Week 14-16:** Write paper/report
- Introduction (problem + motivation)
- Related work
- Our approach
- Experiments + results
- Conclusion
- Prepare presentation

---

## 🤔 Decision Time!

So here's what we need to decide:

### **Questions for Discussion:**

1. **Which option do you prefer?**
   - Option 1: Byzantine + PQC (my recommendation)
   - Option 2: Compact PQC (too hard?)
   - Option 3: Byzantine + Non-IID (no crypto?)
   - Option 4: Privacy + Byzantine (impossible?)

2. **Are we comfortable with math/crypto?**
   - If YES → Option 1 or 2
   - If NO → Option 3

3. **What's our risk tolerance?**
   - Low risk → Option 1 (clear path)
   - High risk → Option 2 or 4 (might fail)

4. **What does "sir" prioritize?**
   - Cryptography → Option 1 or 2
   - Practical FL → Option 3
   - Novel research → Option 2 or 4

5. **Do we have access to GPU/compute?**
   - If YES → Can run more experiments (Option 1)
   - If NO → Need lightweight approach

---

## 📝 Next Steps

**Before we commit, let's:**

1. **All three of us read:**
   - ✅ README.md (big picture)
   - ✅ 01.md (FL fundamentals)
   - ✅ 02.md (why PQC)
   - ✅ 02a.md (quantum + PQC algorithms)
   - ✅ This file (03.md - decision time!)

2. **Meet and discuss:**
   - Which option makes most sense?
   - What are our strengths? (coding? math? crypto?)
   - What resources do we have? (compute? time?)
   - What does sir expect?

3. **Make a decision:**
   - Pick ONE option
   - Commit to it
   - Start working immediately

4. **Week 1 tasks:**
   - Set up development environment
   - Find relevant papers (I can help with this)
   - Start implementing baseline FL system
   - Create GitHub repo for code

---

## 💭 Final Thoughts

Look, I know this seems overwhelming. We have:
- 12 FL challenges
- 8 PQC challenges  
- Multiple trade-offs
- Only 4 months

**But here's the thing:** We DON'T need to solve everything. We need to:
1. Pick ONE clear problem
2. Make ONE meaningful contribution
3. Show it works with experiments
4. Write it up clearly

**My vote:** Option 1 (Byzantine + PQC)

It's the sweet spot of:
- ✅ Interesting (combines two important problems)
- ✅ Realistic (doable in 4 months)
- ✅ Novel (nobody's done this efficiently)
- ✅ Satisfies requirements (cryptography + FL)

But ultimately, it's a team decision. Let's discuss and commit! 🚀

---

**What do you both think? Let's set up a meeting and decide! 💪**

*- Asneem*
