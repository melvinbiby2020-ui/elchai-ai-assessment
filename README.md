# 🤖 AI Agent Research Assessment — Elchai Group
### Meeting Summary & Action Tracker | AI Workflow Prototype

> **Submitted by:** Melvin Biby  
> **Role Applied:** AI Agent & OpenClaw Research Intern  
> **Submission Date:** August 2026  
> **Location:** UAE

---

## 📋 Overview

This repository contains the full pre-interview assessment submission for the **AI Agent & OpenClaw Research Intern** role at Elchai Group, Dubai.

The assessment demonstrates a practical, end-to-end AI agent workflow — a **Meeting Summary & Action Tracker** — built using Claude 3.5 Sonnet (Anthropic), OpenAI Whisper, n8n, and Notion. It includes workflow design, prompt engineering, risk analysis, tool comparison, and a final business recommendation.

---

## 🗂️ Repository Contents

```
📁 elchai-ai-assessment/
├── 📄 README.md                          ← You are here
├── 📄 Melvin_Biby_Elchai_Assessment.pdf  ← Full submission document
├── 📁 workflow/
│   ├── 🔧 n8n_workflow.json              ← Importable n8n workflow (see below)
│   └── 📷 workflow_diagram.png           ← Visual pipeline diagram
├── 📁 prompts/
│   ├── 01_meeting_summarisation.txt      ← Primary Claude prompt
│   ├── 02_action_item_followup.txt       ← Follow-up email prompt
│   └── 03_risk_escalation.txt           ← Risk flagging prompt
└── 📁 sample_output/
    ├── sample_transcript.txt             ← Example meeting transcript (anonymised)
    └── sample_claude_output.json         ← Example JSON output from Claude
```

---

## 🔄 Workflow at a Glance

```
[Meeting Recording]
        ↓
[Whisper API] → Transcription
        ↓
[Claude 3.5 Sonnet] → Summary + Action Items (JSON)
        ↓
[n8n Orchestration] → Schema Validation
        ↓
[Notion Database] → Status: PENDING REVIEW ⚠️
        ↓
[Human Reviewer] → Approve / Edit / Reject
        ↓
[Gmail via n8n] → Summary distributed to participants
        ↓
[Notion] → Status: APPROVED ✅ | Calendar reminders set
```

> ⚠️ **Human-in-the-loop is mandatory.** No action items are distributed without explicit human approval. Auto-approve is disabled by design.

---

## 🧰 Tech Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Transcription | OpenAI Whisper v2 | Audio → timestamped text |
| AI Model | Claude 3.5 Sonnet | Summarisation, extraction, risk flagging |
| Orchestration | n8n (self-hosted) | Workflow automation & routing |
| Output Store | Notion API | Structured action tracker database |
| Distribution | Gmail (via n8n) | Summary delivery to participants |

---

## 🧠 Prompts

### 1. Meeting Summarisation (Primary)

```
System:
You are an expert meeting analyst for a professional services firm.
Produce structured, accurate summaries with no hallucinated content.

User:
Given the following transcript, return a JSON object with fields:
- meeting_title
- date (YYYY-MM-DD)
- participants (list)
- summary (3–5 sentences)
- action_items: [{ task, owner, deadline (YYYY-MM-DD), priority: High/Medium/Low }]
- key_decisions (list)
- open_questions (list)
- risk_flags (list or empty array)

Return null for any field that cannot be determined from the transcript.
Return ONLY valid JSON. Do not add commentary.

TRANSCRIPT:
{transcript_text}
```

### 2. Action Item Follow-up Email

```
Given this list of action items, generate a polite follow-up email
reminding each owner of their tasks and deadline. Keep the tone
professional and concise. Sign off as the meeting organiser.

ACTION ITEMS:
{action_items_json}
```

### 3. Risk Escalation Flag

```
Review the following meeting summary. Identify any items that require
immediate management attention, legal review, or carry financial /
reputational risk. List them clearly with a brief explanation of the concern.

SUMMARY:
{summary}
```

---

## 📊 Tool Comparison Summary

| Criterion | Claude 3.5 Sonnet ✅ | GPT-4o | Gemini 1.5 Pro | Llama 3.1 70B |
|-----------|---------------------|--------|----------------|---------------|
| Summary quality | ★★★★★ | ★★★★★ | ★★★★☆ | ★★★☆☆ |
| JSON reliability | ★★★★★ | ★★★★☆ | ★★★★☆ | ★★★☆☆ |
| Context window | 200K tokens | 128K | 1M tokens | 128K |
| Hallucination risk | Low | Low–Med | Medium | Med–High |
| Data privacy | High (zero-retention opt) | Medium | Medium | Highest (self-hosted) |
| Cost (input/1M tokens) | ~$3 | ~$5 | ~$3.50 | ~$0.35 (cloud) |
| UAE data residency | ⚠️ US-based | ⚠️ US-based | ⚠️ US/EU-based | ✅ Self-hostable |

**Selected:** Claude 3.5 Sonnet — best balance of accuracy, safety, and cost for this use case.

---

## ⚠️ Risk Summary

| Risk | Level | Control |
|------|-------|---------|
| Hallucinated action items | 🔴 High | Human review gate; null fields enforced in prompt |
| Confidential transcript exposure | 🔴 High | HTTPS, zero-retention API config, files deleted post-processing |
| Speaker misidentification | 🟡 Medium | Reviewer checks all names before approval |
| Auto-approval bypass | 🟡 Medium | Hard-blocked in n8n; timeout escalates, never auto-approves |
| UAE PDPL compliance | 🟡 Medium | Legal review recommended; Azure UAE North as fallback |
| Prompt injection via transcript | 🟢 Low | Transcript passed as data field, not raw system prompt |

---

## ✅ Final Recommendation

> **VERDICT: TEST → ADOPT (with conditions)**

Recommended for a **4-week controlled pilot** with one internal team:

- ✅ 3–5 meetings per week
- ✅ 100% human review on every output
- ✅ Weekly error rate tracking (target: < 5%)
- ✅ Reviewer satisfaction survey (target: > 8/10)
- ⚠️ Legal/PDPL review before processing **client** meeting transcripts

---

## 🔒 Known Limitations

- **Simulated workflow:** The activity log is based on expected outputs, not a live production run. A real pilot is needed to validate accuracy with actual Elchai transcripts.
- **OpenClaw not evaluated:** No public documentation or API access was available at the time of submission. A separate evaluation can be conducted once access is provided.
- **Speaker diarisation:** Whisper's speaker identification is imperfect. Pyannote or AssemblyAI would improve accuracy.
- **No live API keys used:** Integration code is ready to execute with real credentials.
- **Notion dependency:** If Elchai uses a different platform (Salesforce, SharePoint), n8n nodes would need reconfiguration (est. 2–3 hours).

---

## 👤 About the Candidate

**Melvin Biby** — BCA in Data Science (IBM Collaboration), Bengaluru, graduated 2026  
📍 UAE (Umm Al Quwain) | Available for on-site work in Dubai

**Skills:** Python · JavaScript · SQL · Node.js · Pandas · NumPy · Machine Learning · Git/GitHub · HTML/CSS  
**Certifications:** IBM Data Science Professional Certificate · Meta Front-End Development (Coursera, Mar 2026)  
**Internship:** Cyber Security Intern (Jan–Feb 2026, AICTE/ICAC approved)

---

## 📎 Submission Files

| File | Description |
|------|-------------|
| [`Melvin_Biby_Elchai_Assessment.pdf`](./Melvin_Biby_Elchai_Assessment.pdf) | Full 9-section assessment document |
| [`workflow/n8n_workflow.json`](./workflow/) | Importable n8n workflow |
| [`prompts/`](./prompts/) | All Claude prompts used |
| [`sample_output/`](./sample_output/) | Example transcript + Claude JSON output |

---

<div align="center">

*Assessment prepared for Elchai Group — AI Agent & OpenClaw Research Intern Application*  
*All content is original work by Melvin Biby, August 2026*

</div>
