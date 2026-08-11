---
name: medical-pdf-evidence-review
description: Review uploaded medical PDFs against current high-quality evidence and create textbook-depth, physician-level study guides with clinical reasoning, practical application, verified citations, Markdown source, a polished Korean-capable PDF, and rendered visual QA. Use when asked to review, explain, annotate, update, fact-check, critique, or create evidence-based educational commentary from a medical PDF, including guidelines, reviews, articles, lecture materials, algorithms, tables, and figures.
---

# Medical PDF Evidence Review

## Primary objective

Create a dense, physician-level study guide that can replace reopening the source PDF for study. Make the completed guide sufficient for learning the source's core content, underlying concepts, pathophysiology, current evidence and guidelines, clinical reasoning, and practical application.

Use this explicit completion standard:

> 완성된 해설서만 읽어도 원본 PDF의 핵심 내용뿐 아니라 해당 주제의 배경지식, 병태생리, 최신 근거, guideline, clinical reasoning과 실제 진료 적용까지 충분히 공부할 수 있어야 한다.

Target family physicians, internists, and relevant specialists. Use Korean for the main explanation unless the user requests otherwise. Retain standard English medical terms, drug names, trial names, and guideline titles when clearer.

Do not produce a short summary or a list of recommendations. Default to a textbook/commentary level of explanation with high learning density. Use substantial length when clinical importance requires it, but never inflate length through repetition.

## Required outputs

Unless the user explicitly requests a different format, complete the full workflow:

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

Write final artifacts to:

```text
output/<topic>-evidence-study-guide.md
output/<topic>-evidence-study-guide.pdf
```

Use a stable, descriptive, filesystem-safe `<topic>` slug. Keep extraction, rendered pages, contact sheets, and other intermediates under `tmp/pdfs/` and exclude them from final deliverables.

## Non-negotiable principles

- Treat the source PDF as evidence to evaluate, never as unquestioned ground truth.
- Read the complete relevant document, including appendices, footnotes, references, tables, figures, flowcharts, and algorithms.
- Verify time-sensitive and management-relevant claims with live, current sources rather than model memory.
- Preserve uncertainty, guideline disagreement, historical context, and subgroup limitations.
- Prioritize clinical applicability, reasoning, decision thresholds, exceptions, and benefit-harm balance.
- Verify citations, DOI, PMID, URLs, page references, numerical values, and recommendation grades. Never invent missing information.
- Do not finish at Markdown. Generate the final PDF, render it, inspect it, correct defects, and render again.

## End-to-end workflow

### 1. Establish scope and inspect every source

1. Identify every PDF relevant to the request. If scope is ambiguous, state a safe assumption or ask only when the choice would materially change the work.
2. Record, when identifiable:
   - title
   - authors or issuing organization
   - publication year and version
   - guideline, review, trial, lecture, or other document type
   - specialty and topic
3. Determine whether the PDF has reliable embedded text. If it is scanned or extraction is unreliable, render pages and use OCR or visual inspection. Cross-check important wording, symbols, and numbers against page images.
4. Read the complete relevant document systematically by section. Maintain a coverage record so later sections receive the same attention as earlier sections.
5. If the source is inaccessible, corrupted, password-protected, or materially illegible, report the exact limitation and request a usable copy. Never imply full review.

Use PDF page labels when available. Otherwise identify the viewer/file page convention used. Never invent page, figure, table, or algorithm numbers.

### 2. Build and prioritize the claim inventory

Extract clinically meaningful claims, including:

- definitions, classifications, diagnostic criteria, and differential diagnosis
- screening, case finding, and risk stratification
- treatment indications, targets, thresholds, and management algorithms
- medication or device choice, dose, route, sequencing, and duration
- contraindications, interactions, monitoring, and treatment failure
- surveillance and follow-up intervals
- procedural, escalation, and referral indications
- sensitivity, specificity, predictive values, incidence, prevalence, prognosis, absolute risk, relative risk, NNT, and NNH
- subgroup and special-population recommendations
- management-relevant tables, figures, flowcharts, and algorithms

Assign a depth tier to every meaningful topic:

- **Tier A - Core clinical decisions:** Diagnostic criteria, treatment indications, medication choice, cutoffs, algorithms, surveillance, procedures, and practice-changing evidence. Explain most deeply and allow multiple pages when needed.
- **Tier B - Important supporting concepts:** Background necessary to understand diagnosis, treatment, or prognosis. Explain definitions, mechanisms, and reasoning sufficiently.
- **Tier C - Ancillary material:** Content with little direct effect on clinical decisions. Cover accurately but relatively concisely.

For each priority claim, record internally:

- exact PDF statement and verified location
- target population, setting, and clinical context
- descriptive evidence versus recommendation
- applicable tier
- threshold, dose, interval, comparator, and effect estimate
- external verification requirement
- verification result and best supporting evidence

Do not let a long document become front-loaded. Process by section if needed, then merge into one coherent guide. Do not compress later sections merely because the document is long.

### 3. Decide what requires external verification

Verify externally when any answer is yes:

1. Could the claim have changed since publication?
2. Is it a recommendation or clinical decision?
3. Does it involve a threshold, cutoff, dose, duration, interval, or procedure?
4. Is it controversial, subgroup-dependent, preference-sensitive, or guideline-dependent?
5. Could an incorrect interpretation affect management or patient safety?

State the evidence-search cutoff date in the guide. If live research is unavailable, disclose that limitation prominently and do not describe conclusions as current.

### 4. Search and select evidence

Use this hierarchy approximately, adapting it to the question:

1. Current official clinical practice guidelines or consensus statements from relevant professional societies and authoritative public-health organizations, including relevant Korean bodies, WHO, CDC, NICE, and USPSTF.
2. Cochrane reviews and high-quality systematic reviews or meta-analyses.
3. Large randomized controlled trials and major practice-changing trials.
4. High-quality prospective cohorts and large observational studies.
5. Peer-reviewed narrative reviews or expert consensus only when stronger evidence is unavailable.

Search authoritative primary sources directly. Trace summaries back to the issuing organization or original publication whenever practical. Do not use commercial sites, blogs, SEO pages, news articles, or unsourced summaries as primary evidence when higher-quality sources exist.

When Korean and international recommendations differ, describe both when clinically relevant. Identify the population, setting, update date, recommendation strength, and certainty or evidence level only when the source explicitly provides them. Do not rank a newer narrative review above a current guideline or high-quality trial merely because it is newer.

### 5. Appraise the evidence and classify PDF claims

Assign one classification to each important claim:

- **Supported:** Current high-quality evidence supports it without a material qualification.
- **Mostly supported:** The main statement is correct but needs qualification, subgroup context, or updated detail.
- **Guideline dependent:** Major guidelines differ in a clinically meaningful way.
- **Evidence limited:** Evidence is weak, inconsistent, indirect, imprecise, or mainly expert opinion.
- **Outdated:** It was reasonable at publication but subsequent evidence or guidance changed it.
- **Contradicted:** Current high-quality evidence does not support it.
- **Unable to verify:** Reliable evidence could not be identified or accessed.

Do not convert uncertainty into certainty. Distinguish lack of evidence from evidence of no effect. When sources disagree, explain their populations, endpoints, values, and evidence bases instead of choosing one without justification.

For an older PDF, explicitly distinguish:

- **Correct at the time of publication**
- **Current recommendation**

Check for updates to classification, diagnostic criteria, screening, surveillance, treatment indications, drug selection, new therapies, contraindications, procedural indications, and risk stratification.

### 6. Develop clinical reasoning, not recommendation lists

For Tier A and important Tier B topics, explain as applicable:

- exact definition, classification, and clinical importance
- relevant physiology, pathophysiology, and natural history
- why a test is obtained and how its result changes management
- why a cutoff or threshold is used, including tradeoffs near the boundary
- why a therapy, drug, dose, duration, intervention timing, or surveillance interval is chosen
- benefit-harm balance, patient values, feasibility, and competing risks
- causal chain from mechanism and evidence to recommendation and bedside decision

Do not end reasoning with "the guideline recommends it." Explain why the recommendation exists and when its logic no longer applies.

For each major study that materially informs practice, explain when relevant:

- Population
- Intervention or exposure
- Comparator
- Outcome and follow-up
- Effect size with precision and absolute effect when available
- key limitations, external validity, and subgroup issues
- practical meaning for current care

Do not merely list study names or article titles.

### 7. Verify numerical and statistical claims

Double-check important cutoffs, diagnostic accuracy, effect estimates, risk percentages, doses, durations, intervals, and procedural thresholds against the original source. Preserve units, denominator, time horizon, endpoint definition, comparator, confidence interval, and population.

If guidelines use different thresholds, do not average or blend them. State which guideline uses which value, for which population and setting, the evidence or rationale, and the practical consequence.

Do not equate statistical significance with clinical importance, association with causation, surrogate outcomes with patient-important outcomes, or relative risk reduction with absolute benefit.

Report absolute risk, absolute risk reduction, NNT, or NNH when provided or when derivable transparently from compatible event counts and time horizons. Show inputs and rounding for derived values. Do not derive them when required information is missing.

### 8. Interpret tables, figures, and algorithms

For each clinically important visual element, explain:

1. what it shows
2. how to interpret it
3. how it affects clinical decisions
4. limitations and excluded populations
5. whether current evidence still supports it

Inspect every management-relevant branch of an algorithm. Reference the exact verified PDF location.

## Required topic structure

Use the following structure for every Tier A topic and adapt it proportionally for Tier B. Keep Tier C concise.

## [주제]

### 1. PDF에서 말하는 내용

State the source claim accurately with verified PDF page, figure, table, or algorithm references.

### 2. 먼저 알아야 할 배경

Explain the definition, classification, clinical importance, relevant physiology, and natural history needed to understand the decision.

### 3. 병태생리 및 Clinical Reasoning

Explain the causal and decision logic in depth. Connect pathophysiology to tests, thresholds, treatment choices, timing, duration, monitoring, surveillance, and benefit-harm balance. Do not abbreviate this section into a few generic sentences.

### 4. 현재 근거와 최신 Guideline

Synthesize current guidelines and higher-level evidence. State recommendation strength or evidence certainty only when explicitly reported.

### 5. 주요 연구 해설

Include this section when trials or observational evidence materially affect practice. Explain PICO, effect size, limitations, and clinical meaning rather than listing studies.

### 6. 실제 임상에서는 어떻게 적용하는가?

Give a concrete, sequential approach. Address applicable items:

- eligible and ineligible patients
- initial evaluation and test selection
- cutoff, threshold, and interpretation near boundaries
- treatment indication, agent, dose, route, and duration
- monitoring, response assessment, and adverse-effect surveillance
- treatment failure, next step, escalation, and referral
- older adults, pregnancy, kidney or liver dysfunction, major comorbidity, and other special populations

Do not abbreviate this section. Make it usable during real clinical decision-making while preserving patient-specific judgment.

### 7. PDF와 최신 근거 비교

Give the classification label and explain whether the PDF is consistent, partially consistent, guideline-dependent, evidence-limited, outdated, contradicted, or unverifiable. State the management consequence.

### 8. 헷갈리기 쉬운 부분 / Clinical Pitfalls

Address common misconceptions, misapplication, guideline differences, controversies, evidence gaps, exceptions, contraindications, and easily missed bedside points.

### 9. Clinical Pearl

End with one to three high-yield sentences that can be remembered and applied.

Use compact tables for exact multi-guideline or multi-threshold comparisons, but retain narrative reasoning for nuance.

## Final synthesis sections

End the study guide with all of the following:

## 핵심 Take-home Messages

Provide 5-10 high-impact conclusions.

## Practice-changing Points

Highlight differences that could materially change diagnosis, treatment, monitoring, referral, or counseling compared with the PDF.

## PDF에서 수정해서 기억해야 할 내용

List outdated, incomplete, misleading, and controversial statements with source locations and corrected interpretations.

## Guideline 간 주요 차이

Compare clinically meaningful disagreements without averaging incompatible recommendations.

## Evidence Gaps

Identify weak evidence, indirectness, unresolved subgroup questions, and research needs.

## Clinical Pearls

Consolidate the most useful bedside lessons without repeating whole sections.

## Key References

Prioritize current guidelines, systematic reviews or meta-analyses, and practice-changing randomized trials.

## Markdown and PDF production

### Generate the Markdown source

Write a complete, self-contained Markdown study guide before PDF production. Include:

- title and source-document identification
- evidence-search and writing cutoff date
- scope and access limitations
- a navigable heading hierarchy
- all topic commentary and final synthesis sections
- verified references with direct links

Check that no section contains placeholders, internal tool tokens, unsupported citations, or incomplete notes.

### Generate a designed study-guide PDF

Create the PDF from the completed content using the bundled PDF-capable runtime and a layout method that supports polished typography, tables, page headers/footers, and pagination. Prefer ReportLab or another reliable local engine over a raw browser print when it provides better control.

Make the PDF look like a study guide, not a plain Markdown printout. Include as applicable:

- cover with document title
- reviewed source PDF information
- evidence-search/writing cutoff date
- table of contents
- clear heading hierarchy and section transitions
- readable body size, line spacing, margins, and paragraph spacing
- well-fitted tables and callout boxes for key points
- page numbers and consistent headers or footers
- Take-home Messages, Practice-changing Points, Clinical Pearls, and References

Discover and embed a Korean-capable font. Verify glyph coverage rather than assuming a font works. Do not deliver a PDF with tofu boxes, substituted glyphs, or broken Korean text.

### Render and QA every final PDF

After generation:

1. Reopen the PDF and verify page count and extractable text where applicable.
2. Render every page to PNG using Poppler or equivalent.
3. Inspect the complete document using contact sheets plus full-size views of dense, table-heavy, or suspicious pages. Do not treat a sample of early pages as complete QA.
4. Check:
   - Korean glyph integrity
   - clipped or missing text
   - table overflow or unreadably small cells
   - heading/body overlap
   - broken citations, URLs, or reference formatting
   - awkward page breaks and orphaned headings
   - excessive blank space
   - body text that is too small
   - incorrect or missing page numbers
   - inconsistent headers, footers, spacing, or hierarchy
5. Correct every material defect, regenerate the PDF, and repeat rendering and inspection.

A PDF's existence is not proof of completion. Complete only when the latest rendered pages have no material visual or formatting defects.

## Citation integrity

For external evidence, provide as many verified fields as available:

- organization or first author
- title
- journal or issuing body
- year
- DOI
- PMID
- direct official, journal, PubMed, or DOI URL

Place citations close to the claims they support. Verify that every link resolves to the cited source and that the source supports the nearby statement. Prefer the latest official guideline version and note corrections, retractions, or updates when found.

Never fabricate a reference, PMID, DOI, title, year, recommendation grade, page number, figure/table label, or URL. Omit any field that cannot be verified. Do not cite search-result pages as evidence.

## Completion criteria

Before reporting completion, confirm all of the following:

- The complete relevant PDF was reviewed, or every limitation is explicit.
- Tier A, B, and C prioritization is internally consistent.
- Later sections are not compressed because of document length.
- Tier A topics explain background, pathophysiology, clinical reasoning, current evidence, practical application, exceptions, and comparison with the PDF at sufficient depth.
- Major studies are interpreted with population, intervention/exposure, comparator, outcomes, effect size, limitations, and clinical meaning when relevant.
- Priority claims have verified PDF locations and current external evidence.
- Numerical values, units, populations, comparators, and time horizons match their sources.
- Guideline differences are presented without averaging incompatible recommendations.
- Current recommendations are distinguished from historical correctness.
- External citations are real, direct, and support the text.
- Both Markdown and PDF outputs exist at the required paths.
- The final PDF was rendered in full and the latest render passed visual QA.
- The completed study guide satisfies the primary objective quoted at the top of this skill.

Treat any of the following as failure:

- summarizing PDF sections in only a few lines
- listing recommendations without explaining why
- listing study names without interpreting their design, results, limitations, and meaning
- providing a very short Clinical Reasoning or practical-application section for a Tier A topic
- writing early sections in depth while compressing later sections
- stopping at Markdown without creating the PDF
- creating a PDF without rendered QA
- increasing length through unnecessary repetition instead of learning density

The goal is not a long summary. Produce an evidence-based, clinically applicable, physician study guide.
