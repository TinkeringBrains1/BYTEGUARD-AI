# Federated Learning Mamba SSM with supervised contrastive learning

**A Next-Generation AI Firewall powered by Federated Learning, Mamba State Space Models, and Supervised Contrastive Learning.**

> **Research Concept:** Traditional Transformers are too slow $(O(N^2))$ for real-time packet filtering. This project utilizes **Mamba (State Space Models)** for linear-time $O(N)$ inference, trained via **Federated Learning** to preserve privacy, and refined with **Supervised Contrastive Learning (SCL)** to detect zero-day SQL injection patterns.

##  The Tech Stack
| Component | Technology | Why? |
| :--- | :--- | :--- |
| **Backbone** | **Mamba-SSM (30M)** | 5x faster inference than BERT; handles long sequences efficiently. |
| **Privacy** | **Federated Learning** | Trains on distributed client data without raw SQL logs leaving the source. |
| **Optimization** | **Supervised Contrastive Loss** | Pushes "Attack" vectors apart from "Normal" vectors in embedding space. |
| **Deployment** | **FastAPI + Async** | Real-time blocking of malicious requests. |

##  How to Run

### Step 1: Train the Model (`fed-mamba-scl.ipynb`)
Open this notebook in **Google Colab (T4 GPU)** or Kaggle.
1.  Upload your dataset (`train_ssm.csv`).
2.  Run all cells.
3.  **What happens:**
    * Simulates 3 Federated Clients.
    * Trains a Mamba model using SCL + CrossEntropy.
    * **Output:** Generates a `model_state_dict.pth` file.
4.  **Action:** Download the `.pth` file and the `tokenizer` folder.

### Step 2: Deploy & Test (`mamba-test.ipynb`)
Open this notebook (can run on local GPU or Colab).
1.  Upload the `model_state_dict.pth` you trained in Step 1.
2.  Run all cells.
3.  **What happens:**
    * **The Server:** Starts a background FastAPI app acting as a "Vulnerable Bank API".
    * **The Firewall:** Intercepts every request, runs it through Mamba, and blocks it if the `Attack Score > 0.85`.
    * **The Attack Sim:** Automatically fires `Generic`, `Obfuscated`, and `Stealthy` SQL injections to test robustness.

##  Results
* **Accuracy:** ~99.2% on Test Set.
* **Inference Latency:** ~45ms per request (vs 120ms with BERT).
* **Detection:** Successfully blocks obfuscated payloads like `1' OR '1'='1` which bypass regex filters.

##  Project Files
* `fed-mamba-scl.ipynb`: The Research & Training Kernel.
* `mamba-test.ipynb`: The Production Simulation Kernel.
