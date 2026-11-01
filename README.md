# Prompt-Assembly-for-Healthcare-Applications

**Student:** Faraz  
**Supervisor:** Dr. Jinan Fiaidhi  
**Institution:** Department of Computer Science, Lakehead University (F/W 2026–2027)

---

## 1. Core Research Question & PICO Study Framing
> *To what extent do modular prompt assembly techniques (dynamic few-shot exemplars, negative guardrails, linguistic shifting, and PICO-structured clinical reasoning) improve the factual reliability, formatting consistency, and hallucination resistance of Small Language Models (`IBM Granite 3.1`) when transforming ambient clinical dialogue into structured SOAP notes?*

* **[P] Patient / Problem:** Unstructured, noisy ambient doctor-patient dialogues containing casual small talk, colloquial idioms, and omitted clinical parameters.
* **[I] Intervention:** Modular Prompt Assembly Pipeline (Delimiters + `MedSynth` Dynamic Few-Shot + Linguistic Shifting + Negative Guardrails + PICO Chain-of-Thought).
* **[C] Comparison:** Baseline Zero-Shot monolithic prompting (`Condition A`).
* **[O] Outcome:** Quantitative gains across Tier 1 (ROUGE / BERTScore), Tier 2 (Medical NER F1 & PICO Completeness), and Tier 3 (Negative Guardrail Compliance & Hallucination Suppression).

---

## 2. Repository Directory Structure (Course Deliverables)

| Directory | Deliverable Front | Contents & Status |
| :--- | :--- | :--- |
| 📁 **[`/Readings`](./Readings)** | **Scoping Review** | Literature matrix synthesizing *Patil et al. (2024)*, *Liu et al. (2025)*, *Zaghir et al. (2024)*, and *Wang et al. (2025)*. |
| 📁 **[`/Coding`](./Coding)** | **Prototyping & Programming** | 4 modular Jupyter Notebooks (`.ipynb`) compatible with Google Colab & Kaggle T4 GPUs, plus Phase 1 evaluation results. |
| 📁 **[`/IEEE_Paper`](./IEEE_Paper)** | **IEEE Paper Writing** | Phase 1 Technical Progress Report, architecture diagrams, and manuscript drafts. |

---

## 3. Google Colab / Kaggle Notebooks (`/Coding`)

| Stage | Notebook | Description | Google Colab Link |
| :--- | :--- | :--- | :--- |
| **Stage 1** | `01_Dataset_Prep_and_Custom_EdgeCases.ipynb` | Ingests `MTS-Dialog` & `MedSynth` and synthesizes the **50-Case Controlled Experimental Dataset** across 10 specialties. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FarazHeydar/Prompt-Assembly-for-Healthcare-Applications/blob/main/Coding/01_dataset_prep_and_custom_edgecases.ipynb) |
| **Stage 2** | `02_MedSynth_FewShot_Retriever.ipynb` | Dense vector semantic indexing (`all-MiniLM-L6-v2`) to retrieve Top-2 condition-matched SOAP exemplars. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FarazHeydar/Prompt-Assembly-for-Healthcare-Applications/blob/main/Coding/02_medsynth_fewshot_retriever.ipynb) |
| **Stage 3** | `03_Granite_Before_vs_After_Pipeline.ipynb` | Runs `ibm-granite/granite-3.1-2b-instruct` across **Condition A (Before)** and **Condition B (After + PICO)**. | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FarazHeydar/Prompt-Assembly-for-Healthcare-Applications/blob/main/Coding/03_granite_before_vs_after_pipeline.ipynb) |
| **Stage 4** | `04_Three_Tier_Evaluation_Engine.ipynb` | Computes Tier 1 (ROUGE/BERTScore), Tier 2 (Entity F1/Linguistic Shift/PICO), and Tier 3 (Guardrails/Hallucinations). | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/FarazHeydar/Prompt-Assembly-for-Healthcare-Applications/blob/main/Coding/04_three_tier_evaluation_engine.ipynb) |
