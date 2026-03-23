# Dynamic-Agentic-Pandas-Engine-with-LLM-Guardrails

A secure AI system that uses a **local Large Language Model (LLM)** to translate natural language queries into **safe executable pandas code**.

The system includes a **security guard layer** that ensures only authorized analytical queries are executed, preventing unsafe or malicious code generation.

---

# Features

* Local LLM inference (Phi-3.5 Mini Instruct)
* Natural Language → Pandas Code generation
* Secure execution environment
* Authorization layer for query validation
* AST-based code validation
* Fully offline system

---

# System Architecture

The system is composed of three main layers:

---

## 1. Authorization Layer (Security Guard)

An LLM determines whether a user query is safe to execute.

It blocks:

* Raw data access (df.head, df.tail)
* Code injection attempts
* File/system access
* Loops or function definitions

Example:

User Input:

```id="a1"
get five rows
```

Output:

```json id="a2"
{"authorized": false}
```

---

## 2. Code Generation Layer

If the query is authorized, the LLM generates **a single line of pandas code**.

Example:

```id="a3"
result = df["salary"].mean()
```

Strict constraints:

* Only one line
* No imports
* No functions
* Must store output in `result`

---

## 3. Execution Layer

The generated code is:

1. Validated using AST parsing
2. Executed in a sandboxed environment
3. Returns only the computed result

---

# Example Usage

```python id="b1"
userQ = "average salary"
fullsystem(df, userQ, SYSTEM_PROMPT_CODE, AUTHORIZED_SYSTEM_PROMPT)
```

Output:

```id="b2"
1683.33
```

---

# Blocked Query Example

```python id="b3"
userQ = "show first 5 rows"
```

Output:

```id="b4"
Unauthorized
```

---

# Not allowed Query Example

```python id="b5"
userQ = "what is the sum of salaries?"
```

Generated Code:

```id="b6"
result = df["salary"].sum()
ValueError: The function is not in allowed function
```

---

# Dataset Schema Awareness

The system dynamically injects dataset schema into the prompt:

```id="b7"
{
 "columns": ["name", "department", "salary", "years_experience"]
}
```

This ensures:

* No hallucinated columns
* Accurate code generation

---

# Installation

```bash id="c1"
pip install torch transformers accelerate bitsandbytes pandas
```

---

# Model

This project uses:

Phi-3.5 Mini Instruct (local)

Example path:

```id="c2"
/content/drive/MyDrive/Phi_3_5_mini_instruct
```

---

# Security Mechanisms

* LLM-based authorization layer
* AST validation
* Restricted execution environment
* Function whitelist (mean, max, count, avg)

---

# Key Concepts

* LLM Code Generation
* Secure AI Systems
* Prompt Engineering
* Sandboxed Execution
* AI Guardrails
* DataFrame Query Systems

---

# Architecture
* This system dynamically converts natural language into safe executable pandas code using a multi-layered LLM architectur.
  
```  
User Query (English / Arabic)
        │
        ▼
  ┌──────────────────────┐
  │  Authorization LLM   │  ──→  Checks if query is SAFE or NOT
  │   (Security Guard)   │
  └──────────────────────┘
        │
        ├───────────────► ❌ Unauthorized → Block Request
        │
        ▼
  ┌──────────────────────┐
  │   Code Generator LLM │  ──→  Generates ONE line pandas code
  │    (Dynamic Engine)  │
  └──────────────────────┘
        │
        ▼
  ┌──────────────────────┐
  │   AST Validator      │  ──→  Validates code structure & safety
  └──────────────────────┘
        │
        ▼
  ┌──────────────────────┐
  │   Secure Executor    │  ──→  Executes code in sandbox
  └──────────────────────┘
        │
        ▼
      Result
```

---

# Future Improvements

* Support groupby queries
* Add visualization layer
* Expand function whitelist safely
* Deploy as REST API
* Add logging & monitoring

---

