# medical-pdf-evidence-review

Codex skill for reviewing medical PDFs against current high-quality evidence and producing textbook-depth, physician-level study guides.

PDFs are treated as sources to evaluate rather than unquestioned ground truth. The workflow prioritizes current clinical practice guidelines, systematic reviews, meta-analyses, and major randomized controlled trials.

## Capabilities

- Reads the complete relevant PDF, including tables, figures, algorithms, appendices, and footnotes.
- Prioritizes content as Tier A core decisions, Tier B supporting concepts, and Tier C ancillary material.
- Verifies recommendations, thresholds, doses, durations, intervals, and numerical outcomes.
- Classifies claims as Supported, Mostly supported, Evidence limited, Guideline dependent, Outdated, Contradicted, or Unable to verify.
- Distinguishes historical correctness from current recommendations.
- Explains pathophysiology, clinical reasoning, benefit-harm balance, and bedside application in depth.
- Interprets major studies using population, intervention/exposure, comparator, outcomes, effect size, limitations, and clinical meaning.
- Produces Korean physician-level commentary with direct, verified citations by default.
- Generates both a complete Markdown source and a designed study-guide PDF.
- Renders every final PDF page and requires visual QA before completion.
- Preserves uncertainty and explains clinically meaningful guideline disagreements.

## Default workflow

```text
Complete PDF review
-> priority claim inventory
-> current evidence verification
-> long-form physician study guide
-> Markdown generation
-> designed PDF generation
-> full PDF rendering and QA
-> final file report
```

Default artifacts:

```text
output/<topic>-evidence-study-guide.md
output/<topic>-evidence-study-guide.pdf
```

## Install

Copy this repository into your personal Codex skills directory:

```bash
git clone https://github.com/cruxism0218-tech/medical-pdf-evidence-review.git ~/.codex/skills/medical-pdf-evidence-review
```

Because this is a private repository, cloning requires GitHub access to `cruxism0218-tech/medical-pdf-evidence-review`.

## Use

Invoke the skill explicitly:

```text
Use $medical-pdf-evidence-review to turn this medical PDF into a textbook-depth Korean physician study guide, verify it against current evidence, and deliver QA-checked Markdown and PDF outputs.
```

The skill may also trigger automatically for requests to review, annotate, update, fact-check, or explain medical PDFs using current evidence.

## Files

```text
medical-pdf-evidence-review/
├── SKILL.md
├── README.md
├── .gitignore
└── agents/
    └── openai.yaml
```

## Clinical safety

This skill supports evidence review and professional education. Its output does not replace patient-specific clinical judgment, local policy, specialist consultation, or applicable regulatory requirements.
