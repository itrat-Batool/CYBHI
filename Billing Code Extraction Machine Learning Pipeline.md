# **AI-Driven Audio → SOAP → Billing Code Extraction Pipeline**

## **Overview**

This document explains the end-to-end architecture used to convert classified staff audio conversations into structured documentation (e.g., SOAP notes) and extract billing codes. The system leverages PII-safe processing, retrieval-augmented generation (RAG), human-in-the-loop validation, and a continuous-learning ML pipeline.

The goal is to use GenAI now, while simultaneously building a proprietary ML model that improves over time.

# **Pipeline Components**

## **1\. Classified Staff → Application**

Users speak into the application, generating audio input that contains sensitive operational information. This audio is captured securely and prepared for downstream processing.

## **2\. PII Redaction**

Before any external computation, the system performs **PII redaction** on transcripts to ensure:

- Compliance (HIPAA, PHI minimization, contractual privacy constraints)  
    
- No sensitive identifiers leak into external LLM calls or storage  
    

PII redaction removes or masks personally identifiable information such as names, dates, addresses, and medical record numbers.

## **3\. RAG (Retrieval-Augmented Generation)**

The cleaned transcript is fed into a **RAG pipeline**, which combines:

- The transcript (facts from the conversation)  
    
- Relevant knowledge (clinical guidelines, SOAP format templates, billing rules)  
    

RAG ensures that outputs remain grounded in authoritative knowledge while reflecting the specifics of the conversation.

## **4\. LLM (Large Language Model)**

The LLM is responsible for:

- Interpreting the transcript  
    
- Structuring information into SOAP or other documentation formats  
    
- Suggesting billing-related entities (codes)  
    

At this stage, the system produces a **draft**.

## **5\. Refine Stage**

The “Refine” component merges RAG contextualization and LLM reasoning to produce an improved draft. Refinement may include:

- Terminology corrections  
    
- Consistency checks  
    
- Formatting into structured SOAP output  
    
- Extracting features needed for billing classification  
    

## **6\. Human-in-the-Loop (HIL) Review**

Human reviewers (e.g., CHW - Community Health Worker) inspect and correct:

- SOAP note accuracy  
    
- Coding interpretations  
    
- Ambiguous or borderline cases  
    
- Any hallucinations or missing information from the AI  
    

This ensures high-quality, auditable outputs suitable for billing workflows.

## **7\. Training Dataset Generation**

All HIL-corrected outputs are stored in a structured **Training Dataset**, which becomes a high-value, domain-specific corpus for future ML training. Each entry typically contains:

- Audio text  
    
- Redacted version  
    
- Model output  
    
- Human-corrected output  
    
- Metadata (context, specialties, billing labels)  
    

## **8\. ML Model Training**

Over time, the training dataset is used to build and fine-tune proprietary ML models capable of:

- Automatic SOAP generation  
    
- Billing code prediction  
    
- Classification of medical intents  
    
- Clinical entity recognition  
    

The ML models eventually reduce reliance on external LLM APIs, saving cost and improving domain accuracy.

## **9\. Feedback Loop**

The ML model outputs are fed back into the **Refine** process, where they serve as:

- Model-generated suggestions  
    
- Candidate structured outputs  
    
- Pre-classified billing codes  
    

Human reviewers continue to correct outputs, expanding the training dataset. This forms a continuous-improvement reinforcement loop.

# **Benefits of This Architecture**

### **Privacy-first**

Sensitive data is redacted before touching external AI services.

### **Human-in-the-loop reliability**

Ensures clinical safety, billing accuracy, and auditability.

### **RAG grounding**

Reduces hallucinations and maintains compliance with medical guidelines.

### **Long-term cost reduction**

Short-term: use GenAI  
Long-term: shift to your own ML models trained on your proprietary dataset.

### **Continuous improvement**

Every review improves the dataset, which improves the model.
