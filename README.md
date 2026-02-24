# PhaseSense
SoberCare – PhaseSense NLP

PhaseSense is a lightweight NLP module used in the SoberCare project to estimate the psychological recovery phase of a user based on their text input. The system analyzes language patterns and returns the most likely behavioral phase.

This tool is designed for supportive insight, not medical diagnosis.

Project Overview

Many recovery support tools rely only on keyword detection or generic chat models. PhaseSense uses a hybrid approach that combines semantic embeddings with lexical signals to interpret user intent more reliably.

The system takes a user message and classifies it into one of five phases commonly observed in addiction recovery communication.

Recovery Phases Detected

Denial / Minimization
The user downplays or dismisses the seriousness of the issue.

Cognitive Negotiation
The user acknowledges the problem but tries to justify limited use.

Active Craving
The user expresses strong urges or desire for substances.

Heightened Vulnerability
Emotional stress or instability that may increase relapse risk.

Recovery / Stabilization
The user shows progress, control, or commitment to recovery.

System Architecture

The model combines two signals:

User Input
   │
   ├── Keyword Matching
   │
   └── Semantic Embedding Search
           │
           ▼
     Hybrid Scoring
           │
           ▼
     Predicted Phase
Core Components
1. Vocabulary Dataset

A structured JSON file defines:

phase-specific keywords

prototype sentences representing typical language patterns

Example structure:

{
  "active_craving": {
    "keywords": [...],
    "prototype_sentences": [...]
  }
}

This dataset acts as the knowledge base for the classifier.

2. Sentence Embeddings

The system converts prototype sentences into vectors using a transformer model.

Current model:

BAAI/bge-small-en

Properties:

Attribute	Value
Vector dimension	384
Type	Sentence Transformer
Purpose	Semantic similarity

These embeddings allow the system to detect paraphrases and indirect language.

3. Vector Store

Embeddings are stored in ChromaDB.

Each record contains:

sentence

embedding

phase metadata

This enables efficient similarity search when a user message arrives.

4. Keyword Scoring

Keywords provide a direct lexical signal.

Example:

deserve → cognitive_negotiation
craving → active_craving
sober → recovery_stabilization

The user message is tokenized and matched against phase vocabularies.

5. Hybrid Decision

The final prediction combines both signals.

final_score =
0.4 × keyword_score
+
0.6 × embedding_similarity

This helps balance precision and generalization.

Repository Structure
PhaseSense/
│
├── vocabulary.json
├── main.py
├── requirements.txt
├── chroma_db/
└── README.md
Installation

Create a virtual environment and install dependencies.

python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
Requirements

Main dependencies:

sentence-transformers
chromadb
numpy
streamlit
google-generativeai

Python version recommended:

Python 3.11
Running the Model

Run the classifier directly:

python main.py

Example output:

Input: I deserve a drink tonight
Predicted Phase: cognitive_negotiation
Scores: {...}
Example Flow

Input:

"I feel like I need a drink."

Processing steps:

Tokenize input

Compute embedding

Retrieve similar prototype sentences

Aggregate phase scores

Combine with keyword signals

Return prediction

Output:

Active Craving
Limitations

The model has several known limitations:

• Negation handling (e.g., “I don’t drink much”) can be ambiguous
• Accuracy depends on vocabulary coverage
• Not designed for clinical decisions
• Context from long conversations is not fully modeled

These are typical constraints for rule-assisted embedding systems.

Future Improvements

Planned improvements include:

conversation context analysis

emotional signal detection

larger prototype dataset

machine learning classifier trained on labeled examples

better negation handling

Intended Use

PhaseSense is intended for:

research prototypes

academic projects

recovery-support experimentation

It should not be used as a diagnostic or medical tool.

License

Educational / research use.
