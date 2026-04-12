# 🧠 Academic Assignment Generator — n8n Workflow

**Autonomous Distinction Engine** for generating submission-ready, distinction-level academic assignments.

## Overview

This n8n workflow implements a full AI-powered pipeline that:

1. Accepts an assignment brief (PDF / DOCX / TXT) via webhook
2. Extracts and analyses all requirements using **Anthropic Claude Sonnet**
3. Generates a brief-driven structure using **OpenAI GPT-4o** (no generic sections)
4. Writes complete, critically analysed content using **OpenAI GPT-4o**
5. Enhances and humanises the output using **Anthropic Claude Sonnet**
6. Validates against the brief using **OpenAI GPT-4o**
7. Auto-refines up to 3 iterations if quality score < 80
8. Outputs a downloadable HTML file with cover page, table of contents, content, and references

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

Send a POST request to the webhook endpoint with the assignment brief as a file upload:

```bash
curl -X POST https://your-n8n-instance/webhook/generate-assignment \
  -F "data=@assignment_brief.pdf"
```

The response will be a downloadable HTML file containing the completed assignment.

## Workflow Nodes (17 total)

| # | Node | Engine | Purpose |
|---|------|--------|---------|
| 1 | Webhook Trigger | n8n | Receives uploaded assignment brief |
| 2 | Consolidate Brief Text | Code | Extracts and validates text from upload |
| 3 | Brief Extraction & Requirement Intelligence | Anthropic Claude | Extracts word count, tasks, marking criteria, hidden expectations |
| 4 | Parse & Validate Extraction | Code | Parses AI response, calculates word allocations |
| 5 | Structure Generation | OpenAI GPT-4o | Builds structure strictly from brief (no templates) |
| 6 | Validate Structure | Code | Filters out any generic sections not in brief |
| 7 | Section Writing Engine | OpenAI GPT-4o | Writes full content: Claim → Evidence → Analysis → Link |
| 8 | Parse Written Content | Code | Parses written sections and references |
| 9 | Critical Analysis Enhancement & Humanisation | Anthropic Claude | Deepens analysis, humanises tone, removes AI patterns |
| 10 | Parse Enhanced Content | Code | Parses enhanced output |
| 11 | Validation Engine | OpenAI GPT-4o | 10-point validation against brief |
| 12 | Parse Validation Results | Code | Determines if refinement needed |
| 13 | Needs Refinement? | If | Routes to refinement loop or final output |
| 14 | Auto-Refinement | Anthropic Claude | Fixes critical issues (max 3 iterations) |
| 15 | Parse Refined Content | Code | Feeds back into validation loop |
| 16 | Generate HTML Document | Code | Creates styled, print-ready HTML with cover page |
| 17 | Respond With HTML File | Webhook Response | Returns downloadable HTML binary |

## Core Principles

- ❌ No generic sections (Introduction, Conclusion, etc.) unless brief explicitly requires them
- ✔ Structure derived only from assignment tasks
- ✔ Minimum 70% peer-reviewed sources
- ✔ MSc-level critical analysis depth
- ✔ Humanised academic tone
- ✔ Auto-refinement loop (up to 3 iterations)