# UNIBA at Cruciverb-IT: Solving Italian Crosswords with Encoder–Decoder Models and Beam Search

The **UNIBA** system for the **Cruciverb-IT** task at **EVALITA 2026** is a lightweight, resource-efficient crossword solver. The code implements a dual-stage pipeline that transitions from individual clue answering to global grid consistency.

### **Code Architecture & Key Components**

The system is built around four primary functional modules:

* **Clue Answering Engine:**
The core is an **encoder-decoder** architecture utilizing **IT5** (a version of the T5 model pretrained specifically for Italian). The code handles input sequences formatted to include the clue text, the target answer length, and—crucially—a "partial solution" string (e.g., `C _ _ S O`) to simulate characters already present in the grid.
* **Dynamic Masking & Training Module:**
The training logic includes a custom data augmentation step where characters in known answers are randomly masked. This allows the model to learn how to refine its predictions based on cross-character constraints found in a crossword grid.
* **Grid Completion via Dynamic Beam Search:**
Instead of a simple greedy fill, the code implements a **dynamic beam search**. As the system iterates through the grid, it updates the list of candidate answers for empty slots, ensuring that new characters placed in "Across" slots are immediately factored into the search for intersecting "Down" slots.
* **Binary Classifier Ranker:**
To select the best final candidate from the beam search, the code includes a classification head. This module is trained to estimate the "correctness" of an answer by analyzing:
* **Structural features:** Character-level fit within the grid.
* **Model-based features:** Confidence scores from the IT5 decoder.

### **Summary Table: System Specifications**

| Component | Technology/Method |
| --- | --- |
| **Model Base** | IT5 (Italian-specific Transformer) |
| **Paradigm** | Encoder-Decoder (Sequence-to-Sequence) |
| **Search Strategy** | Dynamic Beam Search with iterative updates |
| **Ranking Logic** | Binary Classifier (Structural + Probability features) |
| **Constraint Handling** | Length-aware and Partial-character conditioning |
