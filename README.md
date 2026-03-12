# Scale AI Demo Lab — FirstBank AI Assistant Evaluation

A hands-on enterprise AI evaluation lab simulating how **Scale AI** helps organizations test, validate, and improve LLM-powered assistants before production deployment.

Built as part of a deep-dive into enterprise AI reliability, this lab evaluates a fictional bank's AI customer service assistant across accuracy, hallucination risk, ambiguity handling, and compliance safety.

---

## The Scenario

**FirstBank** has deployed an AI assistant to handle customer questions about account policies. Before going live, the enterprise needs to answer:

- Does the AI hallucinate when it doesn't know the answer?
- Does it handle compliance-sensitive questions safely?
- What happens when policy documents conflict?
- What happens without guardrails?

These are the exact questions a Scale AI Solutions Engineer surfaces during a customer engagement.

---

## Lab Structure

```
scale-AI-demo-lab/
│
├── cd_policy.txt                  # FirstBank CD policy document
├── account_dispute_policy.txt     # FirstBank dispute & fraud policy
├── business_account_policy.txt    # FirstBank business account policy
│
├── Prompts_Excel.xlsx             # 20 structured test prompts + results
├── lab_runner.py                  # Automated evaluation pipeline (Python)
│
└── README.md
```

---

## Evaluation Framework

20 prompts structured across 4 failure categories — mirroring how Scale AI approaches enterprise LLM evaluation:

| Category | Prompts | What It Tests |
|---|---|---|
| Baseline | B1–B5 | Normal customer questions — establishes accuracy floor |
| Hallucination Risk | H1–H5 | Questions where the answer isn't in the documents |
| Ambiguity | A1–A5 | Vague questions that require clarification, not guessing |
| Compliance & Safety | C1–C5 | Regulated territory — investment advice, liability, fraud |

---

## Stress Tests

Three additional stress tests beyond the standard evaluation:

**1. No System Prompt**
Removed the grounding instruction to test whether guardrails come from the prompt or the model's own training.

**2. No Documents**
Ran prompts with no policy documents attached — simulating a non-RAG system answering from general internet knowledge.

**3. Conflicting Documents**
Introduced a contradictory policy update to test whether the AI detects conflicts or silently accepts the latest information.

---

## Key Findings

| ID | Finding | Severity |
|---|---|---|
| F1 | No conflict detection — AI accepted contradictory policy update without flagging it | Critical |
| F2 | Retrieval completeness failure — Bank Secrecy Act line dropped from response | High |
| F3 | Confident hallucination without documents — AI invented competitor penalty table | High |
| F4 | Partial compliance failure — AI redirected tax question but still fulfilled the request | High |
| F5 | Accuracy vs. helpfulness gap — correct refusal but no guidance toward valid options | Medium |
| F6 | Context contamination in sequential testing — prompts influenced each other | Medium |

---

## Automation Pipeline

`lab_runner.py` automates the full evaluation loop:

- Reads prompts directly from Excel
- Sends each prompt to the LLM via API in a **fresh conversation** (no context contamination)
- Collects responses automatically
- Writes results back to a new Excel file

```bash
python3 lab_runner.py
```

**Why this matters:** Manual testing introduces context contamination — the AI remembers previous questions and uses them to interpret later ones. The automated pipeline resets conversation state between every prompt, producing accurate and reproducible results.

---

## Concepts Demonstrated

| Concept | How It Appears in This Lab |
|---|---|
| **RAG** (Retrieval Augmented Generation) | Policy documents serve as the grounding knowledge base |
| **Grounding** | Baseline tests verify answers are traceable to source documents |
| **Chunking** | 3 separate policy files enable targeted retrieval by topic |
| **Hallucination Testing** | H1–H5 and no-document stress test surface fabricated responses |
| **Guardrail Evaluation** | C1–C5 test compliance behavior with and without system prompt |
| **Pipeline Failure Analysis** | Findings distinguish retrieval failures from generation failures |
| **RLHF Context** | Findings simulate the human evaluation layer that feeds Scale's RLHF pipeline |

---

## Tech Stack

- Python 3.9
- OpenAI API (GPT-3.5-turbo)
- openpyxl (Excel automation)
- Postman (manual API validation)

---

## How to Run

1. Clone the repo
2. Install dependencies:
```bash
pip3 install openai openpyxl
```
3. Set your OpenAI API key:
```bash
export OPENAI_API_KEY="your-key-here"
```
4. Run the evaluation pipeline:
```bash
python3 lab_runner.py
```
5. Open `Results.xlsx` to review automated responses alongside manual findings

---

## Author

**Vrushali Deo**
Senior Technical Lead | AI & Enterprise Integrations
[LinkedIn](https://linkedin.com/in/vrushali-d-6899114) | [GitHub](https://github.com/vrushalideo)
