# Clinical Anonymization & Annotation Assistant

A web-based, desktop-class clinical text editor designed to assist in identifying, annotating, and redacting Personally Identifiable Information (PII) from physician letters (*Arztbriefe*) and medical reports.

The application operates as a **serverless, browser-only application**. It runs entirely inside the client’s browser, communicating directly with local or external LLM providers and storing your settings securely in browser storage.

<img width="1258" height="843" alt="Bildschirmfoto 2026-06-07 um 16 19 17" src="https://github.com/user-attachments/assets/e3425050-4220-4864-9787-0d14d5b47940" />

---

> [!WARNING]
> ### ⚠️ Critical Disclaimer & Privacy Notice
> 1. **100% User Responsibility**: This software is an assistant tool. Natural Language Processing (NLP) and Large Language Models (LLMs) are subject to hallucinations, omissions, and errors. The user bears **100% of the responsibility** for ensuring that all protected health information is thoroughly and correctly redacted before any document is shared. Manual verification of every output is mandatory.
> 2. **On-Premises Infrastructure Only**: When dealing with real, sensitive, or protected patient data, **only on-premises (local) LLM integrations (such as Ollama running on a secure local network/localhost) should be used**. Sending patient-identifiable medical records to external cloud APIs (such as Google, Groq, or OpenRouter) without appropriate data processing agreements is a severe breach of patient confidentiality and strict medical privacy laws (including GDPR and HIPAA).

---

## Key Features

*   **Serverless / Client-Side Execution**: No custom backend or server-side database required. The app can be hosted directly via static web hosting (like GitHub Pages).
*   **Farbcodierte Annotation (Color-Coded Highlights)**: Categorizes clinical entities into three distinct buckets for visual review:
    *   🔴 **Patient Data** (`patient`): Patient names, dates of birth, addresses, and contact details. Replaced by `[PATIENT]`.
    *   🟢 **Medical Staff** (`staff`): Names of treating physicians, nurses, assistants, or clinic staff. Replaced by `[MED_MITARBEITER]`.
    *   🔵 **Clinical Facility** (`clinic`): Names of hospitals, specialized wards, practices, and physical addresses. Replaced by `[KLINIK]`.
*   **Bulk Anonymization**: A dedicated **"Komplett anonymisieren"** button safely replaces all identified entities with their respective placeholders (`[PATIENT]`, `[MED_MITARBEITER]`, `[KLINIK]`) in a single pass.
*   **Inversion Errechnung (Back-to-Front Redaction)**: Bulk anonymization processes replacements from the end of the document to the front. This prevents text selection offsets from drifting during active substitution.
*   **Local LLM Integration (Ollama)**: Easily connects to a locally hosted instance of Ollama to keep 100% of the data processing on your own machine.

---

## Architecture Overview

```text
 ┌────────────────────────┐         Secure Local Fetch (CORS)      ┌─────────────────────────┐
 │                        │  ───────────────────────────────────>  │   On-Premises LLM       │
 │  Cappuccino Frontend   │                                        │   (Localhost Ollama)    │
 │     (Objective-J)      │  <───────────────────────────────────  │   Runs entirely local   │
 │                        │        Structured JSON Response        └─────────────────────────┘
 │  Runs fully in browser │
 └───────────┬────────────┘
             │
             │ Persistent Settings (No document text is saved here)
             ▼
 ┌────────────────────────┐
 │     Browser Storage    │
 │   (CPUserDefaults /    │
 │     LocalStorage)      │
 └────────────────────────┘
```

---

## Tech Stack

*   **Frontend UI & Layout**: Objective-J, [Cappuccino](https://github.com/cappuccino/cappuccino) (a high-fidelity port of AppKit and Foundation to the browser).
*   **State Management**: Browser LocalStorage.
*   **Inference Connection**: Asynchronous browser `fetch` API directly querying configured endpoints.

---

## Getting Started

### Prerequisites

*   A modern, standards-compliant web browser.
*   **To run locally with Ollama (Recommended for Privacy)**: Ensure your local instance is running and has Cross-Origin Resource Sharing (CORS) enabled so your browser is permitted to query the port:
    ```bash
    OLLAMA_ORIGINS="*" ollama serve
    ```

> [!TIP]
> ### Model Recommendation for German Clinical Texts
> In local testing on Apple Silicon (specifically a **MacBook Pro M2 with 32 GB RAM**), the model **`gemma4:e4b-mlx`** delivered reliable results for identifying and structuring German physician letters (*Arztbriefe*). It is recommended as a starting point for local, private clinical annotation.

### Setup and Running Locally

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/daboe01/GrammarMom2.git
    cd GrammarMom2
    ```

2.  **Start a local static server**:
    Since this is a fully static client-side application, any simple HTTP server is sufficient. For instance, using Python:
    ```bash
    python3 -m http.server 8000
    ```

3.  **Open the application**:
    Navigate to `http://localhost:8000` in your web browser.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
