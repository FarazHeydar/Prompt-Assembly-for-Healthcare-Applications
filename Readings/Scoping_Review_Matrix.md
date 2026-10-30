# Scoping Review: Prompt Engineering & Assembly Paradigms in Clinical Practice
**Course:** COMP9800 MSc Project (F/W 2026–2027)  
**Author:** Faraz | **Supervisor:** Dr. Jinan Fiaidhi  
**Institution:** Department of Computer Science, Lakehead University  

---

## 1. Scoping Review Objective & PICO Research Framing
Following the **PRISMA-ScR** (Preferred Reporting Items for Systematic Reviews and Meta-Analyses Extension for Scoping Reviews) methodology established by *Zaghir et al. (2024)*, this scoping review maps how modular prompt engineering and prompt assembly techniques transform noisy, unstructured clinical narratives into reliable, audit-ready documentation (such as SOAP notes).

### PICO Formulation of the Scoping Review
* **[P] Population / Clinical Problem:** Ambient doctor-patient clinical dialogues, unstructured medical transcripts, and clinical charting workflows prone to conversational noise, colloquial terminology, and LLM hallucinations.
* **[I] Intervention:** Modular Prompt Assembly techniques—specifically **Few-Shot Prompting**, **Chain-of-Thought (CoT) / PICO Reasoning**, **Self-Consistency**, **Generated Knowledge**, **Linguistic Shifting**, and **Negative Guardrails**.
* **[C] Comparison:** Unconstrained zero-shot monolithic prompting across Large and Small Language Models (LLMs/SLMs).
* **[O] Outcome:** Factual reliability (Medical NER Precision/Recall/F1), lexical/semantic alignment (ROUGE, BERTScore), hallucination suppression (Negative Guardrail Compliance), and clinician user satisfaction.

---

## 2. Synthesis of Core Literature & Essential References

### 2.1 Foundational Prompt Engineering Baseline
* **LaunchDarkly (101 Tutorial):** *Prompt Engineering Best Practices* (`https://launchdarkly.com/blog/prompt-engineering-best-practices/`)
  * **Core Contributions:** Establishes structural engineering practices for enterprise LLM deployment, including explicit role assignment, markdown/XML structural delimiters to separate instructions from untrusted user input, output schema enforcement, and iterative prompt refinement.
  * **Application to Our Pipeline:** Directly informs **Module 1 (Structural Delimiters & Noise Filtering)** in our Condition B pipeline (`<TARGET_TRANSCRIPT>` vs. `<STYLE_REFERENCE_ONLY>`).

### 2.2 Deep Analysis of Essential Peer-Reviewed Healthcare References

| Study | Journal & Citation | Prompting Paradigms Evaluated | Clinical Domain & Task | Key Empirical Findings | Identified Research Gap Addressed by Our Project |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1. Patil et al. (2024)** | *Electronics*, 13(15), 2961.<br>"Prompt engineering in healthcare." | Zero-Shot, Few-Shot, Role Prompting, Instruction Tuning | Clinical documentation, diagnostic support, patient triage | Demonstrates that unconstrained LLMs make dangerous logical assumptions when clinical data is incomplete. Shows that explicit role assignment and negative constraints reduce clinical confabulation. | Patil et al. conceptually advocate for negative constraints but do not mathematically quantify **Negative Guardrail Compliance (%)** on omitted clinical parameters in SLMs. |
| **2. Liu et al. (2025)** | *Journal of Medical Internet Research (JMIR)*, 27, e72644.<br>"Prompt engineering in clinical practice: tutorial for clinicians." | Structural Delimiters, Chain-of-Thought (CoT), Linguistic Shifting, Template Formatting | Ambient clinical charting (SOAP & H&P notes), clinical summarization | Provides practical frameworks for clinicians to strip patient small talk and map layperson idioms into standardized clinical taxonomy. Highlights that CoT minimizes diagnostic reasoning gaps in complex case reviews. | Liu et al. provide qualitative clinician tutorials; our work operationalizes their guidelines into an **automated multi-stage Prompt Assembly Pipeline** with quantitative before-vs-after benchmarking. |
| **3. Zaghir et al. (2024)** | *Journal of Medical Internet Research (JMIR)*, 26, e60501.<br>"Prompt engineering paradigms for medical applications: scoping review." | Few-Shot, CoT, **Self-Consistency**, **Generated Knowledge** | Broad medical NLP (EHR extraction, QA, summarization, ICD coding) | Comprehensive scoping review revealing that most medical LLM studies rely on naive zero-shot prompts or proprietary cloud models (GPT-4), with limited investigation of modular prompt assembly on privacy-preserving local SLMs. | Directly motivates our focus on **IBM Granite 3.1 SLMs (2B/8B)** and our evaluation of **Few-Shot context bleed** vs. **PICO-CoT** and **Self-Consistency / Generated Knowledge**. |
| **4. Wang et al. (2025)** | *Frontiers in Artificial Intelligence*, 8, 1517918.<br>"Balancing accuracy and user satisfaction: the role of prompt engineering in AI-driven healthcare solutions." | Multi-constraint Prompting, Hallucination Guardrails, User-Centric Formatting | AI-driven healthcare consultations & clinical summaries | Proves that standard NLP metrics (like ROUGE) fail to capture clinical safety; introduces multi-dimensional evaluation balancing factual precision, formatting consistency, and hallucination resistance. | Inspires our **Three-Tier Evaluation Matrix** (combining Tier 1 ROUGE/BERTScore, Tier 2 Biomedical NER/PICO, and Tier 3 Safety Guardrail Compliance). |

### 2.3 Benchmark & Corpus Literature
* **Ben Abacha et al. (2023 — *EACL*):** *MTS-Dialog (MEDIQA-Chat)* — Provides 1,700 authentic doctor-patient dialogue snippets paired with clinician-authored note sections. Used as our external benchmark.
* **Mianroodi et al. (2025 — *SMM4H*):** *MedSynth (Dial2Note)* — Provides 10,000+ synthetic dialogue-to-SOAP note pairs across 2,000+ ICD-10 codes. Used strictly as our external vector-indexed retrieval pool for Dynamic Few-Shot conditioning.

---

## 3. Taxonomy of the 6 Prompt Assembly Techniques Investigated in COMP9800

| Technique | Operational Mechanism in Clinical Charting | Clinical Charting Use Case | Phase 1 Status & Phase 2 Plan |
| :--- | :--- | :--- | :--- |
| **1. Dynamic Few-Shot Prompting** | Retrieves 1–3 condition-matched SOAP exemplars via dense semantic embeddings (`all-MiniLM-L6-v2`) to anchor note structure. | Standardizing SOAP discharge and consultation summaries. | **Implemented in Phase 1** (+13.94% ROUGE-2 gain; identified 2B SLM context bleed to be resolved via template sanitization in Phase 2). |
| **2. Linguistic Shifting** | System directives instructing the LLM to cross-reference and translate colloquial patient speech into formal medical taxonomy. | Mapping *"heart is fluttering"* $\rightarrow$ *"intermittent palpitations"*; *"peeing razor blades"* $\rightarrow$ *"severe dysuria"*. | **Implemented in Phase 1** (82.00% shift accuracy on Custom 50 corpus). |
| **3. Negative Guardrails** | Strict negative constraints forbidding clinical assumptions and requiring `"Not Documented"` for unmeasured vitals/history. | Preventing fabricated vitals (`RR`, `SpO2`, `Temp`) or assumed `Allergies`. | **Implemented in Phase 1** (Improved compliance from 0.0% to 100.0%, $p < 0.0001$). |
| **4. PICO-Structured CoT** | Forces intermediate step-by-step extraction of **[P]**atient/Diagnosis, **[I]**ntervention, **[C]**omparison/Alternative, and **[O]**utcome. | Structuring differential diagnoses and justifying treatment choices in the Assessment & Plan. | **Implemented in Phase 1** (60.0% completeness; moving to PICO-First CoT ordering in Phase 2 to eliminate token truncation). |
| **5. Self-Consistency** | Samples $N=3$ independent reasoning paths (`temperature=0.7`) and selects the consensus clinical entities/plan via majority vote. | High-stakes risk stratification, medication dosage verification, and drug-interaction checks. | **Scheduled for Phase 2** (Module 6 expansion across `Granite-2B` and `Granite-8B`). |
| **6. Generated Knowledge** | Forces the model to state verified clinical guidelines (e.g., *Ottawa Ankle Rules*, *Centor Criteria*, *CURB-65*) *before* drafting the Plan. | Anchoring Assessment & Plan recommendations to evidence-based clinical guidelines. | **Scheduled for Phase 2** (Module 7 expansion). |
