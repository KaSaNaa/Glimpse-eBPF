# The Solo Developer Strategy

To finish this alone, you must avoid building a full commercial suite. Your goal is to prove **one technical concept**: that an eBPF-based "shared primitive" can intelligently manage telemetry load without losing accuracy.

---

### **6-Month Phase Roadmap**

#### **Phase 1: Environment & "Hello World" (Month 1)**

* **Goal:** Set up your "Lab" and run your first in-kernel program.
* **Action:** Install a Linux VM (Ubuntu 22.04+). Use **libbpf-bootstrap** to avoid complex Makefile configuration.
* **Milestone:** A program that intercepts packets at the XDP layer and prints "Packet Received" to the trace pipe.


* **Research Baseline:** Create a simple eBPF program that counts total packets per Source IP using a standard `BPF_MAP_TYPE_HASH`. This is your comparison point.



#### **Phase 2: The Probabilistic Sketch (Months 2–3)**

* **Goal:** Implement the "TFM" (Traffic Feature Map) using a **Count-Min Sketch (CMS)**.
* **Action:** Replace your heavy Hash Map with a fixed-size array (`BPF_MAP_TYPE_ARRAY`).
* **Technical Challenge:** In eBPF, you cannot use complex loops. You must implement a simple, fast hash function (like Jenkins or Murmur) that the **eBPF Verifier** will accept.


* **Milestone:** A kernel program that updates a 2D grid of counters, tracking millions of packets in just a few kilobytes of memory.



#### **Phase 3: Adaptive Triggering Logic (Month 4)**

* **Goal:** Make the system "intelligent."
* **Action:** Write a user-space Python script that monitors the "fill-rate" or "error-rate" of your sketch.
* **The "Adaptive" Part:** When the script detects high load (e.g., >10k flows/sec), it sends a signal to the kernel to switch from "High Detail" (detailed flow tracking) to "Low Detail" (probabilistic summarizing) to prevent CPU exhaustion.


* **Milestone:** The system automatically "compresses" its data during a simulated traffic spike.

#### **Phase 4: Evaluation & Data Collection (Months 5–6)**

* **Goal:** Prove the "hard work" with numbers.
* **Action:** Use a tool like `pktgen` or `tcpreplay` to feed your system real-world traces (like CAIDA traces).


* **Comparison:** Record the CPU and Memory usage of your **Glimpse-eBPF** vs. the **Baseline** you built in Phase 1.


* **Milestone:** Create graphs showing your system maintains ~95% accuracy while using 1/10th the memory of traditional methods.



---

### **How to "Show You Worked Hard" (Portfolio Evidence)**

Even if your final code has bugs, these three "Artifacts" will prove your research rigor to your markers:

1. **The Verifier Diary:** Keep a log of every time the eBPF Verifier rejected your code. Documenting how you optimized your C code to satisfy the kernel's safety constraints is the definition of "doing the impossible" in this field.


2. **The Methodology Log:** Document your choice of hash functions and why you chose a specific "bucket" size for your sketch. This turns a "coding project" into a "science project".


3. **The "Future Work" Section:** Research is about knowing where you stopped. If you didn't have time for the React dashboard, write a detailed plan for how it *would* have integrated. This shows high-level architectural thinking.



### **Immediate Next Step**

Don't write any code yet.

1. **Clone** [libbpf-bootstrap](https://github.com/libbpf/libbpf-bootstrap).
2. **Compile** the `minimal` example.
3. If it runs, your environment is ready. If not, your first "hard work" task is fixing your Linux kernel headers!
