---
name: Literature Review
description: Guides systematic literature review for academic research. Activates when the user needs to search, screen, synthesize, or write a literature review section — especially for biomedical topics, Traditional Chinese Medicine (TCM), aging biology, or molecular mechanisms. Covers PubMed/PubMed Central search strategy design, PRISMA screening workflow, evidence synthesis, gap analysis, structured writing, citation verification, AI-pattern removal, and final export to Word (.docx) and PDF.
version: 1.1.0
---

# Literature Review Skill

You are a systematic literature review specialist. Help the user conduct rigorous, reproducible academic literature reviews aligned with top-venue standards (NeurIPS, ICML, Nature, Cell, PNAS).

---

## Phase 1 — Scope Definition

Before searching, clarify:
1. **Research question**: Use PICO framework (Population, Intervention, Comparison, Outcome)
2. **Inclusion/exclusion criteria**: Study types, date range, language, species
3. **Target databases**: PubMed, Google Scholar, Web of Science, Semantic Scholar, bioRxiv
4. **Review type**: Narrative, systematic, scoping, or meta-analysis

---

## Phase 2 — Search Strategy

Construct Boolean search strings:
```
("ovarian aging" OR "ovarian reserve" OR "premature ovarian insufficiency")
AND
("traditional chinese medicine" OR "herbal medicine" OR "phytotherapy")
AND
("mechanism" OR "molecular pathway" OR "oxidative stress" OR "mitochondria")
```

For each database, adapt syntax (MeSH terms for PubMed, topic fields for Web of Science).

---

## Phase 3 — Screening & Extraction

Follow PRISMA 2020 flow:
1. **Identification**: Total records retrieved
2. **Deduplication**: Remove duplicates
3. **Title/abstract screening**: Apply inclusion criteria
4. **Full-text review**: Detailed eligibility check
5. **Data extraction**: Standardized table (study, year, population, intervention, outcome, key finding)

---

## Phase 4 — Evidence Synthesis

Organize findings by:
- **Mechanistic themes**: oxidative stress, mitochondrial dysfunction, autophagy, inflammation, epigenetics
- **Active compounds**: flavonoids, alkaloids, polysaccharides, terpenoids
- **Signaling pathways**: PI3K/AKT, Nrf2/HO-1, SIRT1, mTOR, FOXO3a
- **Study quality**: RCT > cohort > case-control > in vitro

---

## Phase 5 — Gap Analysis

Identify:
- Under-studied mechanisms or compounds
- Missing clinical translation (animal → human)
- Methodological limitations (sample size, follow-up duration)
- Conflicting findings requiring resolution

---

## Phase 6 — Writing Guidance

### Introduction Structure
```
1. Clinical burden (epidemiology of ovarian aging / POI)
2. Current treatment limitations
3. Rationale for TCM modernization approach
4. Scope of this review
```

### Related Work / Background Structure
```
1. Molecular mechanisms of ovarian aging
   - Follicle pool depletion
   - Oxidative stress & mitochondrial dysfunction
   - DNA damage accumulation
   - Hormonal dysregulation
2. TCM compounds with documented efficacy
   - Classification by herb category
   - Key active ingredients
3. Known mechanistic targets
4. Gaps and open questions
```

---

## Phase 7 — Citation Verification (文献核实)

After the synthesis draft is complete, run a three-layer citation verification pass before finalizing.

### Layer 1 — Format Check
Verify every reference entry contains all required fields:
- Author(s), Year, Title, Journal/Conference, Volume, Pages/Article ID, DOI
- Flag: missing DOI, year mismatch, truncated author list ("et al." in first author position)

### Layer 2 — Existence Check
For each cited paper, attempt one of the following verifications:
1. Search PubMed by PMID or DOI: `https://pubmed.ncbi.nlm.nih.gov/?term=<DOI>`
2. Search Semantic Scholar API: `https://api.semanticscholar.org/graph/v1/paper/<DOI>`
3. Cross-check via CrossRef: `https://api.crossref.org/works/<DOI>`

Mark each reference with one of:
- ✅ Verified — DOI resolves and metadata matches
- ⚠️ Unverified — URL resolves but title/author mismatch detected
- ❌ Not found — DOI does not resolve or paper does not exist in database

### Layer 3 — Content Accuracy Check
For all ✅ Verified papers, check:
- Does the cited claim match the abstract/conclusions of the paper?
- Is the evidence level correctly labeled ([*in vitro*] / [*animal*] / [*RCT*] / [*human tissue*])?
- Are quantitative values (effect sizes, p-values, sample sizes) accurately transcribed?

### Verification Output Format
Produce a verification table:

| # | First Author | Year | Journal | DOI | Status | Issue |
|---|------------|------|---------|-----|--------|-------|
| 1 | Smith J | 2022 | Nat Med | 10.xxxx | ✅ | — |
| 2 | Li X | 2023 | Phytomed | 10.xxxx | ⚠️ | Title mismatch |
| 3 | Wang Y | 2021 | — | — | ❌ | Cannot locate |

**Action rules**:
- ✅ references: include as-is
- ⚠️ references: correct metadata before inclusion, note the correction
- ❌ references: REMOVE from manuscript and replace with a verified alternative, or flag as [CITATION NEEDED] for the user to resolve manually

---

## Phase 8 — AI Writing Pattern Removal (去AI化润色)

After verification, apply a systematic de-AI pass to eliminate detectable machine-generated writing patterns.

### Patterns to Remove

**Formulaic openers** (replace with direct statements):
- ❌ "It is worth noting that..." → ✅ State the fact directly
- ❌ "It is important to highlight..." → ✅ Delete the preamble
- ❌ "Furthermore, it is evident that..." → ✅ "X demonstrates..."
- ❌ "In conclusion, this study demonstrates..." → ✅ "X demonstrates..."

**Inflation and hedging clusters**:
- ❌ "plays a crucial/pivotal/vital role in" → ✅ "regulates" / "drives" / "is required for"
- ❌ "significant implications for" → ✅ be specific about what the implication is
- ❌ "holds great promise" → ✅ state the specific evidence

**Redundant transition phrases**:
- ❌ "Moreover," / "Furthermore," / "Additionally," at sentence start → delete or rephrase
- ❌ "It should be noted that" → delete
- ❌ "As mentioned above" → reference the specific finding instead

**Symmetry and list overuse**:
- Avoid three-part constructions: "X, Y, and Z" appearing more than twice per paragraph
- Avoid consecutive sentences with identical grammatical structure

**Unnatural precision**:
- ❌ Citing round percentages without source: "studies have shown a 50% improvement"
- ✅ Always anchor quantitative claims to specific cited evidence

### Sentence-Level Rewriting Principles

1. **Vary sentence length**: Mix short (< 15 words) declarative sentences with longer analytical ones
2. **Use active voice** for claims of causation; passive voice only for methods descriptions
3. **Favor specific nouns over vague nominalizations**: "the activation of..." → "activated"
4. **Preserve the author's intellectual contribution**: rewriting must not alter the scientific meaning

### Fluency Check (academic register)
Ensure the final text:
- Reads naturally at the pace of *Nature Reviews* or *Cell* review articles
- Does not contain consecutive sentences beginning with the same subject
- Uses field-specific terminology precisely (do not substitute synonyms for technical terms)

---

## Phase 9 — Document Export: Word & PDF

After Phases 7 and 8 are complete, generate two publication-ready output files.

### Step 1 — Save Markdown Source
Save the final review as a `.md` file in the current working directory or `/temp`:
```
<title>_final_review.md
```

Use standard academic Markdown:
- `# Title`, `## Section`, `### Subsection` for headings
- `**bold**` for key terms on first introduction
- Tables in GFM (GitHub Flavored Markdown) format
- References section with numbered entries `[1]`, `[2]`, etc.

### Step 2 — Export to Word (.docx)

Check Pandoc availability:
```bash
pandoc --version
```

If available, run:
```bash
pandoc "<title>_final_review.md" \
  -o "<title>_final_review.docx" \
  --reference-doc="$HOME/.claude/templates/academic_reference.docx" \
  --toc \
  --toc-depth=3 \
  -V geometry:margin=2.5cm
```

If no reference template exists, create a plain .docx without `--reference-doc`:
```bash
pandoc "<title>_final_review.md" \
  -o "<title>_final_review.docx" \
  --toc \
  --toc-depth=3
```

Expected Word output structure:
- Title: Heading 1, 14pt, bold
- Section headings: Heading 2 / Heading 3 styles
- Body text: Times New Roman 12pt, 1.5 line spacing
- Tables: auto-formatted with header row highlighted
- References: numbered list, 10pt, hanging indent

### Step 3 — Export to PDF

**Preferred method (LaTeX engine — best typesetting)**:
```bash
pandoc "<title>_final_review.md" \
  -o "<title>_final_review.pdf" \
  --pdf-engine=xelatex \
  -V mainfont="Times New Roman" \
  -V fontsize=12pt \
  -V geometry:margin=2.5cm \
  -V linestretch=1.5 \
  --toc \
  --toc-depth=3 \
  --highlight-style=tango
```

**Fallback method (wkhtmltopdf — if LaTeX unavailable)**:
```bash
pandoc "<title>_final_review.md" \
  -o "<title>_final_review.pdf" \
  --pdf-engine=wkhtmltopdf \
  --toc
```

**Windows-specific notes**:
- On Windows, prefer `--pdf-engine=xelatex` with MiKTeX installed
- If font errors occur, replace `"Times New Roman"` with `"Georgia"` or `"Cambria"`
- Use forward slashes in all paths within bash commands on Windows/WSL
- If `wkhtmltopdf` is not on $PATH, use the full binary path:
  ```bash
  pandoc "<title>_final_review.md" \
    -o "<title>_final_review.pdf" \
    --pdf-engine="/c/Program Files/wkhtmltopdf/bin/wkhtmltopdf.exe" \
    --toc \
    -V margin-top=25mm -V margin-bottom=25mm \
    -V margin-left=25mm -V margin-right=25mm
  ```

### Step 4 — Verify Output Files
```bash
ls -lh "<title>_final_review.docx" "<title>_final_review.pdf"
```

Report file sizes to the user. Expected ranges:
- `.docx`: 50–500 KB for a standard review (< 100 pages)
- `.pdf`: 200 KB – 2 MB depending on tables and figures

### Troubleshooting

| Error | Likely Cause | Fix |
|-------|------------|-----|
| `pandoc: command not found` | Pandoc not installed | `winget install JohnMacFarlane.Pandoc` (Windows) or `brew install pandoc` (macOS) |
| `xelatex not found` | LaTeX not installed | `winget install MiKTeX.MiKTeX` (Windows) or `brew install --cask mactex` |
| `wkhtmltopdf not found` | Not installed or not on PATH | `winget install wkhtmltopdf.wkhtmltox` then use full path |
| Font not found | System font missing | Switch to `"Cambria"` or `"DejaVu Serif"` |
| Table rendering broken | GFM table syntax error | Validate table alignment in Markdown source |
| `.docx` styles missing | No reference template | Run without `--reference-doc` flag |

---

## Output Formats (Complete List)

Produce structured outputs across all phases:
- **Search log**: Database, query string, date, hits
- **PRISMA flow table**: Numbers at each stage
- **Evidence table**: Standardized extraction per paper
- **Synthesis narrative**: Thematic paragraphs with citations
- **Gap map**: Table of open questions by mechanism/compound
- **Citation verification table**: Status per reference (✅ / ⚠️ / ❌)
- **De-AI edit log**: Summary of patterns removed and sentences revised
- **Final files**: `<title>_final_review.md`, `.docx`, `.pdf`

---

## Quality Standards

- All claims must cite primary literature (no review-of-reviews as sole source)
- Use precise language: "associated with" vs "causes", "suggests" vs "demonstrates"
- Note study limitations inline
- Distinguish in vitro, animal, and human evidence
- Flag contradictory findings explicitly
- Zero ❌-status references in the final manuscript
- No AI writing patterns detectable by GPTZero / Originality.ai heuristics

---

## Phase Execution Checklist

Before declaring the review complete, confirm:

- [ ] Phase 1: PICO scope defined and confirmed with user
- [ ] Phase 2: Boolean search strings constructed for ≥ 2 databases
- [ ] Phase 3: PRISMA flow documented with numbers
- [ ] Phase 4: Evidence synthesized with inline evidence-level tags
- [ ] Phase 5: Gap map table produced
- [ ] Phase 6: Writing follows Introduction + Background structure
- [ ] Phase 7: All references verified; ❌ entries removed or replaced
- [ ] Phase 8: AI writing patterns removed; fluency check passed
- [ ] Phase 9: `.docx` and `.pdf` files generated and size-verified
