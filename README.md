# 🧠 Academic Assignment Generator — n8n Workflow

**Autonomous Distinction Engine** for generating submission-ready, distinction-level academic assignments.

## Overview

This n8n workflow implements a full AI-powered pipeline that:

1. Accepts an assignment brief (PDF / DOCX / TXT) via a built-in web form
2. Extracts text from uploaded binary files using **Extract From File**
3. Extracts and analyses all requirements using **Anthropic Claude Sonnet**
4. Generates a brief-driven structure using **OpenAI GPT-4o** (no generic sections)
5. Writes complete, critically analysed content using **OpenAI GPT-4o**
6. Enhances and humanises the output using **Anthropic Claude Sonnet**
7. Validates against the brief using **OpenAI GPT-4o**
8. Auto-refines up to 3 iterations if quality score < 80
9. Outputs a downloadable HTML file with cover page, table of contents, content, and references

## File

- `academic_assignment_generator.json` — Import this into n8n

## Setup

### Prerequisites

- [n8n](https://n8n.io/) (self-hosted or cloud)
- OpenAI API key (GPT-4o access)
- Anthropic API key (Claude Sonnet access)

### Installation

1. Open your n8n instance
2. Go to **Workflows** → **Import from File**
3. Select `academic_assignment_generator.json`
4. Configure credentials:
   - **OpenAI API** — Add your OpenAI API key under the credential named `OpenAI API`
   - **Anthropic API** — Add your Anthropic API key under the credential named `Anthropic API`
5. Activate the workflow

### Usage

1. Activate the workflow in n8n
2. Open the form URL provided by the Form Trigger node (shown in the node's settings)
3. Upload your assignment brief (PDF, DOCX, or TXT) using the file upload field
4. Submit the form and wait for the generated HTML assignment to be returned

The form is accessible at:
```
https://your-n8n-instance/form/generate-assignment
```

## Workflow Nodes (24 total)

| # | Node | Engine | Purpose |
|---|------|--------|---------|
| 1 | Form Trigger | n8n | Presents a web form for uploading the assignment brief |
| 2 | Extract From File | n8n | Extracts text from uploaded binary (PDF/DOCX/TXT) |
| 3 | Consolidate Brief Text | Code | Validates and consolidates extracted text |
| 4 | Brief Extraction Chain | LangChain LLM Chain | Orchestrates the brief extraction AI call |
| 5 | Anthropic: Brief Extraction Model | Anthropic Claude | Model sub-node for brief extraction |
| 6 | Parse & Validate Extraction | Code | Parses AI response, calculates word allocations |
| 7 | Structure Generation Chain | LangChain LLM Chain | Orchestrates structure generation AI call |
| 8 | OpenAI: Structure Generation Model | OpenAI GPT-4o | Model sub-node for structure generation |
| 9 | Validate Structure | Code | Filters out any generic sections not in brief |
| 10 | Section Writing Chain | LangChain LLM Chain | Orchestrates the section writing AI call |
| 11 | OpenAI: Section Writing Model | OpenAI GPT-4o | Model sub-node for section writing |
| 12 | Parse Written Content | Code | Parses written sections and references |
| 13 | Critical Analysis Chain | LangChain LLM Chain | Orchestrates the critical analysis AI call |
| 14 | Anthropic: Critical Analysis Model | Anthropic Claude | Model sub-node for critical analysis |
| 15 | Parse Enhanced Content | Code | Parses enhanced output |
| 16 | Validation Chain | LangChain LLM Chain | Orchestrates the validation AI call |
| 17 | OpenAI: Validation Model | OpenAI GPT-4o | Model sub-node for validation |
| 18 | Parse Validation Results | Code | Determines if refinement needed |
| 19 | Needs Refinement? | If | Routes to refinement loop or final output |
| 20 | Auto-Refinement Chain | LangChain LLM Chain | Orchestrates the auto-refinement AI call |
| 21 | Anthropic: Auto-Refinement Model | Anthropic Claude | Model sub-node for auto-refinement |
| 22 | Parse Refined Content | Code | Feeds back into validation loop |
| 23 | Generate HTML Document | Code | Creates styled, print-ready HTML with cover page |
| 24 | Respond With HTML File | Webhook Response | Returns downloadable HTML binary |

## Core Principles

- ❌ No generic sections (Introduction, Conclusion, etc.) unless brief explicitly requires them
- ✔ Structure derived only from assignment tasks
- ✔ Minimum 70% peer-reviewed sources
- ✔ MSc-level critical analysis depth
- ✔ Humanised academic tone
- ✔ Auto-refinement loop (up to 3 iterations)