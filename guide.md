# The "Minimal Scope" Strategy

To finish this alone, you must be ruthless. Do not build a fancy UI or a full security suite. Focus purely on:

1. **The Kernel Logic:** One robust sketch (Count-Min Sketch) in eBPF.
2. **The Trigger:** A simple "Heavy Hitter" detection (identifying IPs that take up too much bandwidth).
3. **The Evidence:** A PDF report showing how your sketch uses 1/10th the memory of standard methods while maintaining 95% accuracy.

---

### Month-by-Month Roadmap

#### Phase 1: The Foundation (Months 1–2)

**Goal:** Master the environment and build a "Hello World" that actually does something.

* **Action:** Set up a Linux VM (Ubuntu 22.04+ is easiest). Install `clang`, `llvm`, and `libbpf`.
* **The "Hard Work" Artifact:** Use **libbpf-bootstrap**. It provides the Makefile and scaffolding so you don't waste weeks on build errors.
* **Milestone:** Build a program that counts total packets per IP using a standard `BPF_MAP_TYPE_HASH`.
* **Why this is research:** This is your **Baseline**. You need this to prove that your later "Sketch" version is better.

#### Phase 2: Implementing the "Impossible" (Months 3–4)

**Goal:** Build the Count-Min Sketch (CMS).

* **Action:** Replace the simple Hash Map with a 2D Array Map (`BPF_MAP_TYPE_ARRAY`).
* **The Challenge:** Implement 3–4 different hash functions (like Jenkins or Murmur) in C. The eBPF verifier will hate loops, so you must **unroll** them or use fixed iterations.
* **Milestone:** A kernel program that updates a 2D grid of counters every time a packet arrives, using almost zero CPU.

#### Phase 3: The "Trigger" & User-Space (Month 5)

**Goal:** Make the kernel talk to the outside world.

* **Action:** Set a threshold. If an IP's estimated count in the CMS exceeds `X`, send an event to user-space using a `BPF_RINGBUF`.
* **Milestone:** A Python script that prints "WARNING: Possible DDoS from IP [x.x.x.x]" when the kernel detects a heavy hitter.

#### Phase 4: The Evaluation (Months 6–7)

**Goal:** This is where you get your "Research" marks.

* **Action:** Download a public dataset (like **CAIDA** traces). Replay these packets through your code using `tcpreplay`.
* **The Experiment:** Run the "Baseline" (Phase 1) and your "SketchMon" (Phase 2) side-by-side.
* **Measure:** 1. **Memory:** How many MBs does the Hash Map use vs. the Sketch? (Usually 100MB vs 1MB).
2. **Accuracy:** How many "Heavy Hitters" did the Sketch miss compared to the perfect Hash Map?
* **The Artifact:** Create a series of graphs showing these trade-offs.

#### Phase 5: Final Documentation & "Monetization" Pitch (Month 8)

**Goal:** Wrap it up.

* **Action:** Write your thesis.
* **Monetization Angle:** Frame your project as a "Library for Cloud-Native Observability." Even if the code is simple, the *architecture* (TFM - Traffic Feature Maps) is what you would sell or open-source.

---

### How to "Show You Worked Hard" (Your Portfolio)

Even if you don't finish every feature, your supervisors will give you top marks if you show **technical rigor**. Keep these three things as "proof":

1. **The Verifier Log Diary:** Keep a text file of the most frustrating errors the eBPF verifier gave you and the code snippets you used to bypass them. This proves you conquered the "impossible" parts of kernel programming.
2. **The Benchmarking Suite:** A folder of scripts that automatically runs your experiments. This shows you didn't just "guess"—you used the scientific method.
3. **The "Future Work" Section:** Research is a cycle. A solo developer who says "I couldn't finish the ML part, but here is exactly how the next person should do it" is viewed as a mature, professional researcher.

### Your First Step (Today)

Do not read more papers. Do this:

1. **Clone** the [libbpf-bootstrap](https://github.com/libbpf/libbpf-bootstrap) repository.
2. **Run** the `minimal` example.
3. **Modify** it to print the Source IP of every incoming packet.

**Once you see those IPs printing in your terminal, you have officially started your research career.**
