# Bilingual Dictionary Audit Report — `Li-case-study.html`

**File:** `C:\Users\Abbas\Desktop\sara_site\Li-case-study.html`
**Dictionary:** `const de = { ... }` (lines 162–484)
**Total unique keys:** 311
**Total key occurrences:** 321 (10 duplicates across 7 key strings)

---

## 1. DUPLICATE KEYS (same English key appears twice in dictionary)

These 7 keys each appear twice with identical English text and different (or same) German translations. The last occurrence overwrites the first in JavaScript.

| English Key | Lines | Note |
|---|---|---|
| `Consentor - a browser extension` | 359, 454 | Identical repeated entry |
| `Regular sessions with legal experts` | 378, 461 | Identical repeated entry |
| `Developer discovery sessions` | 379, 462 | Identical repeated entry |
| `GDPR document analysis` | 380, 463 | Identical repeated entry |
| `Since any interaction patterns were predefined by regulation, my UX work had to find clarity and trust within those boundaries, not around them.` | 377, 464 | Identical repeated entry |
| `Working within legal constraints` | 376, 465 | Identical repeated entry |
| `Key testing findings & what changed` | 338, 475 | Identical repeated entry |

All 7 are harmless duplicates (same English → same German), but they bloat the dictionary and should be deduplicated.

---

## 2. MISSING KEYS (English text in body with no dictionary entry)

These text nodes appear in the visible page but have **no matching key** in `de`, so they stay English when the user toggles to German.

### 2A. Headings

| English Text | Location (line) | Tag |
|---|---|---|
| `01 - EMPATHIZE` | 20 (side nav ×2), 34 (h2), 105 (h2) | `<h2>`, `<a>` |
| `02 - DEFINE` | 20 (side nav ×2), 68 (h2), 121 (h2) | `<h2>`, `<a>` |
| `How might we statements` | 69 (h3), 123 (h3) | `<h3>` |
| `Legal design constraints` | 75 (h3) | `<h3>` |
| `Key research challenge` (paragraph text after heading) | 41 | `<p>` — only the heading `Key research challenge` is in dict (line 369), **not** the paragraph body |

### 2B. Research methods list items (Consentor section)

| English Text | Line |
|---|---|
| `Product tracking (Grafana): Tracked 1,000+ testers' interactions with Consentor over 3 weeks, monitoring drop-off, errors, and consent completion rate.` | 37 |
| `GDPR legal review: Read GDPR articles and guidelines directly relevant to consent requirements. Collaborated with legal experts to understand what users must legally see and decide.` | 38 |
| `Competitive analysis: Reviewed real examples of cookie consent implementations, cataloguing patterns, good and manipulative.` | 39 |

Only the first list item (`User surveys...`) has a dictionary entry (line 368). The other 3 are missing.

### 2C. "How might we" statements (Consentor section)

| English Text | Line |
|---|---|
| `How might we explain GDPR consent categories in plain language so users can make genuinely informed choices?` | 71 |
| `How might we give users meaningful control over their data without overwhelming them with choices at every touchpoint?` | 72 |

The "How might we" statements for the Panels section (lines 125–127) ARE in the dictionary (lines 466–468). But these two for Consentor are NOT.

### 2D. Legal design constraints (all 4 items)

| English Text | Line |
|---|---|
| `Consent must be freely given - no pre-checked boxes, no "accept all" as the only visible option` | 77 |
| `Withdrawal must be as easy as giving consent - "Reject" must be as prominent as "Accept"` | 78 |
| `Clear and plain language required - GDPR Article 7(2) mandates that consent requests must be understandable` | 79 |
| `Purpose-specific consent - users must be able to consent by category (analytics, marketing, etc.), not just blanket accept` | 80 |

### 2E. Pain point paragraphs (Consentor — section 01-EMPATHIZE)

| English Text | Line |
|---|---|
| `Users encounter cookie banners several times per day and have developed a habit of dismissing them without reading, defeating the purpose of consent.` | 49 |
| `GDPR-required terminology (e.g. legitimate interest) is incomprehensible to the average user, making consent meaningless in practice.` | 53 |
| `Many existing implementations deliberately hide "Reject" option or pre-select "Accept". Users don't realise their choices are being manipulated.` | 57 |
| `Users must re-consent across every website separately. There is no single place to manage or review all consent decisions.` | 61 |
| `Users cannot see which third parties their data has been shared with, or what specifically has been collected under their consent.` | 65 |

The pain point **headings** (`Consent fatigue`, `Legal language`, `Dark patterns`, `No centralised control`, `No transparency`) ARE in the dictionary (lines 371–375). But the **paragraphs** under them are NOT.

### 2F. Paragraphs under panels research methods

| English Text | Line |
|---|---|
| `For Cookie banner customer panel & Auditing panel as legally regulated compliance tools, traditional user research answers only half the question. Before I could understand what publishers need, I had to understand what GDPR requires, because those requirements shaped every field, every flow, and every decision in both panels.` | 105 |
| `I went through GDPR articles, mapping each legal obligation to a design requirement. What must a publisher disclose? What options can they have? What is legally mandatory vs. optional?` | 110 |
| `Understanding technical constraints early: which GDPR checks could be automated by scanning a website, and which required manual publisher input.` | 114 |
| `Studied EU guidance documents, real compliance case studies, and existing tools to understand the problem landscape beyond just the legal text.` | 118 |

The research methods **headings/labels** (`Regular sessions with legal experts`, `Developer discovery sessions`, `GDPR document analysis`) ARE in the dictionary. But the **descriptive paragraphs** under each are NOT.

### 2G. "Finding:" and "Fix:" standalone labels (structural issue)

| English Text | Locations (line) |
|---|---|
| `Finding:` | 92, 96, 100, 137, 141, 146 |
| `Fix:` | 93, 97, 101, 138, 142, 147 |

These appear 6 times each as standalone text nodes inside `<b>` tags. The dictionary has **no standalone** `"Finding:"` or `"Fix:"` keys.

**Critical structural bug in the Consentor section (lines 92–101):**
The `<b>Finding:</b>` tag creates separate text nodes. The dictionary keys include `"Finding:"` as a prefix (e.g. `"Finding: Participants did not notice..."`), but the DOM splits this into two text nodes: `"Finding:"` and `" Participants did not notice..."`. **Neither node matches any dictionary key**, so the entire Consentor Findings & Fixes section (lines 92–101) is completely untranslatable.

The panels section (lines 137–147) is partially affected — the paragraph text after `</b>` DOES match dictionary keys, but the `"Finding:"`/`"Fix:"` labels themselves do not.

### 2H. Card tool text mismatch

| English Text | Line | Issue |
|---|---|---|
| `Figma, Notion, SoSci Survey, Prolific, Grafana` | 18 | The `<p>` text is just the tool names. The dictionary has `"Tools: Figma, Notion, SoSci Survey, Prolific, Grafana"` (line 352) with a `"Tools: "` prefix. No match. |

### 2I. Research challenge paragraph

| English Text | Line |
|---|---|
| `The biggest operational challenge was tracking interactions for 1,000+ EU testers. I solved this by filtering tester IDs in SoSci Survey and Grafana dashboard based on certain dates, identifying users who had stopped interacting mid-flow, then contacting those specific users on Prolific to collect bug reports directly talking to them, sharing findings with developers in Notion in real time.` | 41 |

The heading `"Key research challenge"` IS in the dictionary (line 369). The paragraph below it is NOT.

---

## 3. SPURIOUS KEYS (dictionary entries not used in this page's body)

The dictionary contains **~240 keys** that do not appear as text nodes in this page's body. These are entries from the index page (`index.html`) and the IoT case study (`ifm-case-study.html`). While they cause no errors, they bloat the dictionary and could confuse maintenance.

Notable unused grou  s:

### Index page content (not on this page):
- `"Sara Gilda Amirhajlou — UX Research, AI UX & Product Design"`
- `"About"`, `"Experience"`, `"Work"`, `"Skills"`, `"Contact"`
- `"Explore my work"`, `"Available for meaningful collaboration"`
- `"My focus"`, `"My method"`, `"My design belief"`
- All skill/capability entries (`"UX Research"`, `"User interviews"`, `"Persona development"`, etc.)
- `"Selected case studies"`, `"Two full UX stories..."`, `"I separated the case studies..."`

### IoT case study content (not on this page):
- `"ifm statmath GmbH · Industrial IoT UX"`
- `"How I optimized Time-on-Task and reduced error rate for an industrial IoT platform?"`
- `"Reduced Time-on-Task for wizard configuration by 25%"`
- `"Industrial monitoring web App"`
- All IoT-specific findings and fixes (`"Finding: Operators misread "Save & Exit" as losing progress."`, etc.)
- `"Design principles derived from research"` and its sub-items
- `"Wizard"`, `"Key design decisions"`, `"Unified severity colour system"`, `"Side panel for AI assistant"`
- All IoT job/timeline entries

### Navigation variants not on this page:
- `"WhatsApp Sara"`, `"WhatsApp me"`, `"WhatsApp directly"`
- `"Ready to collaborate"` (without `?`)
- `"Open to meaningful UX research, product design, and human-centered AI experience work."` (without `/UI`)

---

## 4. WHITESPACE & CHARACTER ISSUES

### 4A. Finding/Fix structural split (critical)

As noted in §2G, the `<b>` wrapping causes text node fragmentation. The dictionary expects single strings like:

```
"Finding: Participants did not notice the \"Manage preferences\" option..."
```

But the DOM produces two text nodes:

```
Text Node 1: "Finding:"     [inside <b>]
Text Node 2: " Participants did not notice the \"Manage preferences\" option..." [after </b>]
```

**Neither matches.** This is the most impactful bug — 6 Finding/Fix pairs across the page are affected.

### 4B. HTML entity decoding

All `&amp;` → `&` decodes correctly in both the DOM and dictionary keys. No mismatch issues detected for standard entities.

### 4C. Quote characters

The quote text at line 31 uses curly/smart quotes `"` / `"` (`\u201C`/`\u201D`):
```html
<p class="quote">"I have to accept the cookies..."</p>
```

The dictionary key at line 367 also uses the same curly quote characters. They match correctly.

Some dictionary entries use escaped straight quotes `\"` in the JSON string (e.g. line 476: `"Finding: Participants did not notice the \"Manage preferences\" option..."`). These should match DOM text nodes where `&quot;` has decoded to `"`.

### 4D. Em dashes & special characters

The page uses Unicode `—` (U+2014 EM DASH) in several places (e.g., "L&I Technology GmbH · GDPR UX" with `·`). These match the dictionary correctly as they use the same Unicode characters.

---

## 5. SUMMARY

| Metric | Count |
|---|---|
| Total unique dictionary keys | 311 |
| Total body text nodes (non-icon, non-number) | ~70 |
| Keys used on this page | ~71 |
| Spurious keys (not on this page) | ~240 |
| Duplicate key strings | 7 |
| Missing English texts (no translation) | ~25+ text strings |
| Structural text-split bugs (Finding/Fix) | 6 pairs affected |

### Priority fixes

1. **HIGH** — `01 - EMPATHIZE` and `02 - DEFINE` headings (appear multiple times) need dictionary entries
2. **HIGH** — All Consentor Finding/Fix sections need restructuring (either remove `<b>` tags or add standalone `"Finding:"` / `"Fix:"` keys and change dictionary pattern)
3. **MEDIUM** — All pain point paragraphs (5 items) need dictionary entries
4. **MEDIUM** — Legal design constraints list items (4 items) need dictionary entries
5. **MEDIUM** — How might we statements for Consentor (2 items) need dictionary entries
6. **MEDIUM** — Research methods list items (3 of 4) need dictionary entries
7. **LOW** — Remove 7 duplicate key entries
8. **LOW** — Remove ~240 spurious keys (index/IoT content) to reduce dictionary size
