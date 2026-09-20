# ✈️ Smart Airport Passenger Assistance Multimodal Chatbot

A multimodal AI chatbot developed for the **MSc Artificial Intelligence** programme to provide smart passenger assistance in a simulated airport environment.

The system accepts **image, voice, and text inputs**, understands passenger queries, retrieves relevant information from an airport knowledge base, and combines multiple modalities through a weighted fusion strategy.

> **Airport:** Nova International Airport
> **Project Type:** MSc Artificial Intelligence — Multimodal Chatbot
> **Primary Environment:** Google Colab with GPU
> **Interface:** Gradio
> **Knowledge Base:** 20 structured airport records

---

## 📌 Project Overview

The **Smart Airport Passenger Assistance Multimodal Chatbot** is designed to demonstrate an end-to-end multimodal AI workflow for airport passenger support.

The chatbot can process:

* 🖼️ Airport images and signs
* 🎙️ Voice questions
* 💬 Typed passenger queries
* 🖼️ + 💬 Image and text together
* 🖼️ + 🎙️ Image and voice together

The system identifies relevant airport services or locations and retrieves information such as:

* Location/service name
* Category
* Terminal
* Floor or zone
* Description
* Directions
* Opening hours
* Accessibility information
* Related facilities
* Assistance contact
* Similarity/retrieval score
* Heuristic confidence level

The project is implemented as a **proof-of-concept academic prototype** rather than a production airport information system.

---

# 🧠 System Architecture

```text
                         USER INPUT
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
       IMAGE                VOICE               TEXT
          │                   │                   │
        CLIP               Whisper             MiniLM
          │                   │                   │
          │              Transcript                │
          │                   │                   │
          │                 MiniLM                 │
          │                   │                   │
          ▼                   ▼                   ▼
   CLIP KB Embeddings     MiniLM KB Embeddings
          │                   │                   │
          └──────────────┬────┴───────────────────┘
                         │
                  Multimodal Fusion
                         │
             Weighted Similarity Routing
                  Image = 0.55
                  Text = 0.45
                         │
                         ▼
                Airport Knowledge Base
                         │
                         ▼
                 Ranked Top-K Results
                         │
                         ▼
                 Unified Response
                         │
          ┌──────────────┼──────────────┐
          │              │              │
       Location       Guidance       Confidence
       /Service       /Hours          /Score
```

### Fusion Strategy

The project uses a **rule-based routing and weighted similarity approach**.

For combined image and text/voice input:

```text
Fused Score =
    0.55 × Image Similarity
  + 0.45 × Text/Voice Similarity
```

CLIP and MiniLM embeddings are maintained in their separate embedding spaces.

```text
Image → CLIP image embedding
      → CLIP knowledge-base embeddings

Text → MiniLM text embedding
Voice → Whisper transcription → MiniLM text embedding
      → MiniLM knowledge-base embeddings
```

CLIP image embeddings are **not directly compared with MiniLM embeddings**.

---

# 🗂️ Project Structure

```text
airport_multimodal_chatbot/
│
├── data/
│   ├── images/
│   │   ├── raw/
│   │   ├── processed/
│   │   └── samples/
│   │
│   ├── audio/
│   └── text/
│
├── knowledge_base/
│   ├── airport_knowledge_base.json
│   └── airport_knowledge_base.csv
│
├── notebooks/
│   └── MultiModelChatbot.ipynb
│
├── src/
│   └── Core Python components
│
├── models/
│   ├── vision/
│   ├── text/
│   ├── fusion/
│   └── configuration files
│
├── evaluation/
│   ├── task2_3/
│   ├── task4_1/
│   ├── task4_2/
│   ├── task4_3/
│   ├── task5/
│   └── task6/
│
├── outputs/
│   ├── task2/
│   ├── task3/
│   ├── task4/
│   ├── task5/
│   └── task6/
│
├── screenshots/
│   ├── task2/
│   ├── task3/
│   ├── task4/
│   ├── task5/
│   └── task6/
│
├── requirements.txt
├── README.md
└── LICENSE
```

---

# 🛠️ Technologies and Models

| Component               | Technology                     |
| ----------------------- | ------------------------------ |
| Programming Language    | Python                         |
| Development Environment | Google Colab                   |
| Hardware                | GPU / CUDA where available     |
| Vision Model            | CLIP ViT-B/32                  |
| Speech-to-Text          | OpenAI Whisper Base            |
| Text Encoder            | Sentence Transformers / MiniLM |
| Vector Retrieval        | FAISS / semantic similarity    |
| Data Processing         | NumPy, Pandas                  |
| Computer Vision         | Pillow, OpenCV                 |
| Audio Processing        | Librosa, SoundFile, FFmpeg     |
| Evaluation              | Scikit-learn, JiWER            |
| Visualisation           | Matplotlib, Seaborn            |
| User Interface          | Gradio                         |
| Optional Deployment     | Docker                         |

---

# 📊 Project Dataset

## Visual Dataset

The project contains a curated airport-signage dataset covering nine visual categories:

```text
gate
baggage_claim
check_in
security
restroom
lounge
transport
information_desk
restaurant
```

The visual dataset contains **225 images**, with **25 images per category**.

The dataset is used for:

* Visual exploration
* Class distribution analysis
* Image preprocessing
* CLIP zero-shot classification
* Top-1 and Top-3 evaluation
* Visual similarity analysis
* Image-to-knowledge-base retrieval

### Visual Dataset Limitations

The dataset is suitable for a proof of concept but does not fully represent real airport conditions.

Important limitations include:

* Synthetic/procedural examples
* Limited camera viewpoint variation
* Limited motion blur
* Limited occlusion
* Lighting and reflection differences
* Limited multilingual signage
* Limited representation of crowded airport environments

---

# 🎙️ Voice and Text Dataset

The project contains a passenger-query dataset covering common airport intents.

Example intents include:

```text
gate
baggage_claim
check_in
security
lounge
transport
information_desk
flight_delay
```

Additional knowledge-base categories are also supported where appropriate, including:

```text
restaurant
lost_and_found
accessibility
opening_hours
passport_control
customs
restroom
prayer_room
```

Voice queries are processed through:

```text
Audio
  ↓
Whisper
  ↓
Transcription
  ↓
MiniLM
```

The same semantic text-processing approach is used for typed text and voice-transcribed text.

---

# 🗃️ Airport Knowledge Base

The chatbot uses a fictional airport called:

**Nova International Airport**

The knowledge base contains **20 structured records**.

Each record contains fields such as:

```text
record_id
name
category
terminal
floor_zone
description
opening_hours
direction
accessibility
related_facilities
emergency_assistance_contact
search_terms
```

Example records include:

```text
KB001 — Gate B12
KB002 — Gate A7
KB003 — Terminal 1 Check-in Area
KB004 — Terminal 2 Check-in Area
KB005 — Baggage Claim Hall
KB006 — Oversized Baggage Service
KB007 — Security Control Terminal 1
KB008 — Security Control Terminal 2
KB009 — Information Desk
KB010 — Lost and Found Office
KB011 — Airport Lounge
KB012 — Prayer Room
KB013 — Restroom Terminal 1
KB014 — Restroom Terminal 2
KB015 — Train Station Connection
KB016 — Taxi Pickup Zone
KB017 — Restaurant Area
KB018 — Special Assistance Desk
KB019 — Passport Control
KB020 — Customs Area
```

The knowledge base is a structured information resource rather than a trained machine-learning model.

---

# ⚙️ Main Project Tasks

The project follows the assignment workflow.

## Task 1 — Environment Setup

The environment establishes the libraries and tools required for multimodal AI development.

Major dependencies include:

* PyTorch
* TorchVision
* Transformers
* Whisper
* Librosa
* Scikit-learn
* Pandas
* NumPy
* Matplotlib
* FAISS
* Gradio
* Pillow
* OpenCV

The project uses reproducibility controls including a fixed random seed.

---

## Task 2 — Data Acquisition and Exploration

### Task 2.1 — Visual Data

Includes:

* Dataset loading
* Image inspection
* Category annotation
* Class distribution
* Similar visual examples
* Dissimilar visual examples
* Dataset limitations
* Metadata and provenance

### Task 2.2 — Voice and Text Data

Includes:

* Passenger query creation
* Intent labelling
* Text exploration
* Vocabulary analysis
* Entity identification
* Voice query generation
* Whisper transcription
* Speech transcription comparison

### Task 2.3 — Airport Knowledge Base

Includes:

* 20-record airport knowledge base
* JSON storage
* CSV inspection copy
* Schema validation
* Basic retrieval testing

---

# 🔧 Task 3 — Preprocessing

## Image Preprocessing

The image pipeline includes:

* Image validation
* Resizing
* Tensor conversion
* ImageNet normalization where applicable
* Conservative augmentation
* Stratified train/validation splitting
* PyTorch Dataset/DataLoader preparation

The CLIP inference pipeline uses the official CLIP processor separately to avoid double normalization.

## Audio Preprocessing

The audio pipeline includes:

* Loading audio
* Conversion to mono
* Standardisation to 16 kHz
* Silence trimming
* Waveform inspection
* Spectrogram visualisation
* Optional MFCC analysis
* Whisper transcription

## Text Preprocessing

The text pipeline includes:

* Lowercasing
* Cleaning
* Tokenisation
* Stop-word handling where appropriate
* Intent labels
* Entity extraction
* Query-level train/validation splitting
* MiniLM embedding generation

Typed and voice-transcribed text use the same semantic processing approach.

---

# 🤖 Task 4 — Model Design

## Vision Model

The project uses:

**CLIP — `openai/clip-vit-base-patch32`**

CLIP is used as a frozen model for:

* Airport sign category recognition
* Image embeddings
* Visual similarity
* Image-to-knowledge-base retrieval

No CLIP fine-tuning is required.

## Speech Model

The project uses:

**Whisper Base**

Whisper is used as a frozen speech-to-text model.

```text
Audio → Whisper → Passenger Text
```

## Text Model

The project uses:

**`sentence-transformers/all-MiniLM-L6-v2`**

MiniLM provides:

* Semantic text embeddings
* Intent similarity
* Knowledge-base semantic retrieval

The model remains frozen.

## Multimodal Fusion

The system supports:

```text
Image only
Text only
Voice only
Image + Text
Image + Voice
```

The final multimodal response is produced through weighted similarity and routing.

---

# 📈 Task 5 — Training and Evaluation

Pretrained CLIP and Whisper models are used as frozen models, so the evaluation focuses on system performance rather than unnecessary model fine-tuning.

## Vision Evaluation

The vision pipeline evaluates:

* Top-1 accuracy
* Top-3 accuracy
* Similarity scores
* Per-class performance
* Confusion matrix
* Correct predictions
* Incorrect predictions

## Speech Evaluation

The speech pipeline evaluates:

* Example transcriptions
* Word Error Rate (WER)
* Exact-match rate
* Mean WER
* Median WER
* Transcription error examples

Potential limitations such as synthetic speech and limited real-world acoustic variation are documented.

## Text Intent Evaluation

The text pipeline evaluates:

* Accuracy
* Precision
* Recall
* Weighted F1
* Macro F1
* Confusion matrix

Typed and voice-derived text can be evaluated separately where the dataset supports this comparison.

## Knowledge-Base Retrieval

The retrieval pipeline evaluates:

* Top-1 retrieval accuracy
* Top-3 retrieval accuracy
* Similarity scores
* Correct retrieval examples
* Failed retrieval examples

Ground-truth record IDs must correspond to the actual knowledge-base records.

## Multimodal Evaluation

The system is evaluated using:

```text
Image only
Text only
Voice only
Image + Text
Image + Voice
```

The evaluation records:

* Final matched record
* Top-3 candidates
* Image result
* Text/voice result
* Fused result
* Similarity score
* Modality agreement
* Heuristic confidence
* Correctness where ground truth is available

---

# 💻 Task 6 — Deployment and User Testing

A professional **Gradio** prototype provides the user interface.

The interface includes:

* Image upload
* Voice recording
* Audio upload
* Text input
* Chat history
* Chatbot response panel
* Top-3 retrieval results
* Similarity score
* Heuristic confidence
* Whisper transcription
* Error handling
* Uncertainty handling

### Supported Interaction Modes

```text
Text
Voice
Image
Image + Text
Image + Voice
```

The prototype is designed for demonstration and academic user testing.

It should be described as a **proof-of-concept prototype**, not a production airport information service.

---

# 🧪 User Testing

At least five structured scenarios are required:

| Scenario   | Input         |
| ---------- | ------------- |
| Scenario 1 | Text only     |
| Scenario 2 | Voice only    |
| Scenario 3 | Image only    |
| Scenario 4 | Image + Text  |
| Scenario 5 | Image + Voice |

For each scenario record:

* Input modality
* User input
* Expected response
* Actual response
* Correctness
* Similarity score
* Confidence
* Observed limitation
* Suggested improvement

User-testing results are stored in the evaluation directory.

---

# ⚠️ Uncertainty Handling

The system does not treat similarity scores as calibrated probabilities.

Confidence is based on a **heuristic similarity/ranking mechanism**.

When the system is uncertain, it should communicate this clearly.

Example:

> “I am not confident enough to identify the requested airport location or service. Please provide a clearer image or more specific query.”

The system should avoid presenting uncertain information as a definite airport instruction.

---

# 🔐 Ethical and Regulatory Considerations

The project considers ethical and regulatory issues related to multimodal passenger assistance.

Important considerations include:

### GDPR and Privacy

Potentially sensitive inputs may include:

* Voice recordings
* Images
* Boarding passes
* Passenger names
* Flight numbers
* Booking information

The system should follow principles such as:

* Data minimisation
* Purpose limitation
* Appropriate consent
* Secure storage
* Limited retention

### Speech Bias

Speech recognition may perform differently depending on:

* Accent
* Pronunciation
* Language
* Background noise
* Audio quality

The current synthetic voice dataset does not provide sufficient evidence for real-world accent and noise robustness.

### Accessibility

The interface should support passengers with different accessibility needs.

Relevant knowledge-base information includes accessibility details for airport services.

### Multilingual Support

The current proof of concept is primarily designed around English passenger queries and English-oriented semantic prompts.

International airport deployment would require broader multilingual testing.

### False Certainty

Airport information such as gates, services, opening times, and operational status can change.

A real deployment should connect to official airport information systems and provide human or official-source handover for critical information.

---

# 📁 Important Output Files

The project generates evidence and evaluation artifacts in folders such as:

```text
evaluation/
outputs/
screenshots/
models/
```

Typical outputs include:

```text
vision_results.csv
vision_class_metrics.csv
speech_results.csv
speech_summary.csv
text_intent_results.csv
text_intent_classification_report.csv
text_retrieval_results.csv
multimodal_results.csv
overall_results.csv
user_testing_results.csv
```

Generated visual evidence may include:

```text
confusion matrices
accuracy charts
similarity charts
speech examples
retrieval examples
multimodal comparison charts
user-testing screenshots
```

---

# 🚀 Installation

Clone the repository:

```bash
git clone https://github.com/MianNouman1836/airport_multimodal_chatbot.git
cd airport_multimodal_chatbot
```

Create a virtual environment if running locally:

```bash
python -m venv .venv
```

### Windows

```bash
.venv\Scripts\activate
```

### Linux/macOS

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Whisper requires FFmpeg.

Verify:

```bash
ffmpeg -version
```

---

# ☁️ Google Colab

The primary development environment for this project is Google Colab with GPU support.

Recommended workflow:

1. Open `MultiModelChatbot.ipynb` in Google Colab.
2. Select a GPU runtime.
3. Run the environment setup cells.
4. Execute the tasks sequentially.
5. Confirm that the project directories are available.
6. Run the preprocessing and modelling cells.
7. Run evaluation cells.
8. Launch the Gradio prototype.

The notebook is designed to demonstrate the complete workflow from environment setup through deployment.

---

# ▶️ Running the Prototype

The final prototype uses Gradio.

The interface supports:

```text
Image upload
Voice recording/upload
Text input
Image + Text
Image + Voice
```

After launching the application, use the generated Gradio URL to interact with the chatbot.

Example text query:

```text
Where is the security control?
```

Example multimodal interaction:

```text
Upload a security-related airport image
+
Ask: "Where is the security control?"
```

The response should contain the matched airport record and relevant information from the knowledge base.

---

# 🐳 Docker

Docker is **optional** for this academic project.

If Docker support is included, the repository should contain:

```text
docker/
├── Dockerfile
├── requirements.txt
└── README.md
```

The Docker instructions should only be used when the repository contains a standalone application entry point compatible with the Dockerfile.

Example:

```bash
docker build -t airport-chatbot -f docker/Dockerfile .
docker run -p 7860:7860 airport-chatbot
```

---

# 📚 Academic Submission Guidelines

This repository supports the working prototype and supplementary material for the academic submission.

### Submission Guidelines

* Upload your submission as a **single file (PDF or DOC)** on the BSBI portal.
* The report must include an **architecture diagram of the full multimodal airport chatbot pipeline**.
* Include **data exploration outputs** and visual dataset analysis.
* Include **airport knowledge base schema documentation**.
* Include evidence of the **image, audio, and text preprocessing pipelines**.
* Explain the **model design**, including CLIP, Whisper, MiniLM, knowledge-base retrieval, and multimodal fusion.
* Include **training/evaluation results for each major component**.
* Include clearly labelled **tables summarising all evaluation metrics**.
* Include **screenshots of the user interface and prototype**.
* Include **examples of correct and incorrect predictions** where available.
* Discuss **system performance, failure cases, uncertainty, dataset limitations, and possible improvements**.
* Include a critical discussion of **ethical and regulatory considerations**, particularly GDPR, privacy, accessibility, speech bias, multilingual limitations, and false certainty.
* **Harvard referencing must be used for all citations** in the academic report.
* A **working prototype**, such as the Google Colab notebook or Python project files, must be provided with clear instructions for running:

  * image processing
  * speech recognition
  * text processing
  * knowledge-base retrieval
  * multimodal fusion
  * user interface
* Any supplementary:

  * source code
  * datasets
  * screenshots
  * model/configuration files
  * evaluation outputs
  * generated figures
  * additional resources

  used in the project must be **submitted or clearly linked in the appendix**.

---

# ✅ Final Submission Checklist

Before submission, verify:

### Report

* [ ] Report is between **2,500 and 3,000 words**
* [ ] Architecture diagram included
* [ ] Data exploration included
* [ ] Knowledge-base schema documented
* [ ] Preprocessing evidence included
* [ ] Model design explained
* [ ] Multimodal fusion explained
* [ ] Vision evaluation included
* [ ] Speech/WER evaluation included
* [ ] Text/intent evaluation included
* [ ] Knowledge-base retrieval evaluation included
* [ ] Multimodal evaluation included
* [ ] Deployment screenshots included
* [ ] Five user-testing scenarios included
* [ ] Limitations discussed
* [ ] Ethical/GDPR considerations discussed
* [ ] Harvard references included

### Prototype

* [ ] Image upload works
* [ ] Voice recording/upload works
* [ ] Text input works
* [ ] Image + text works
* [ ] Image + voice works
* [ ] Chatbot response panel works
* [ ] Retrieval score is displayed
* [ ] Uncertainty message is displayed when appropriate
* [ ] Top-3 retrieval results are displayed
* [ ] Knowledge-base information is returned from the structured KB

### Evaluation

* [ ] Vision Top-1 measured
* [ ] Vision Top-3 measured
* [ ] Speech WER measured where reference transcripts exist
* [ ] Intent precision/recall/F1 measured where labels support it
* [ ] Retrieval Top-1 measured
* [ ] Retrieval Top-3 measured
* [ ] Multimodal scenarios evaluated
* [ ] Correct and incorrect examples documented
* [ ] No fabricated metrics
* [ ] Ground-truth labels verified
* [ ] Evaluation outputs saved

### Reproducibility

* [ ] Requirements file included
* [ ] Random seed documented
* [ ] Model names documented
* [ ] Dataset structure documented
* [ ] Run instructions included
* [ ] Output directories documented
* [ ] Configuration files included
* [ ] Colab notebook included

---

# ⚠️ Important Project Limitations

This system is an academic proof of concept using a fictional airport knowledge base and relatively small datasets.

It should not be interpreted as a real airport operational information system.

In particular:

* The knowledge base contains simulated airport information.
* The visual dataset has limited real-world diversity.
* Voice data is limited and largely synthetic.
* Real airport noise and accent robustness require further testing.
* Airport operational information is not connected to live airport systems.
* Similarity-based confidence is heuristic rather than calibrated probability.
* Multilingual and accessibility coverage would require further development.
* A production system would require stronger security, privacy, monitoring, validation, and official data integration.

---

# 🔮 Future Improvements

Potential future development includes:

* Larger real-world airport image datasets
* Real passenger voice recordings
* Noise-robust speech recognition
* Multilingual passenger support
* OCR for airport signs
* Live airport flight/gate information
* Official airport APIs
* Learned multimodal fusion
* Calibrated confidence estimation
* Human-agent handover
* Stronger accessibility support
* Secure production deployment
* Automated model monitoring
* Larger and independently verified user-testing datasets

---

# 📌 Academic Purpose

This repository demonstrates the complete multimodal AI development workflow required for the project:

```text
Environment
     ↓
Data Acquisition
     ↓
Data Exploration
     ↓
Preprocessing
     ↓
Model Design
     ↓
Vision + Speech + Text
     ↓
Knowledge-Base Retrieval
     ↓
Multimodal Fusion
     ↓
Training & Evaluation
     ↓
Deployment
     ↓
User Testing
     ↓
Ethical & Regulatory Analysis
```

The project is intended for **academic demonstration and evaluation** as part of an MSc Artificial Intelligence assignment.
