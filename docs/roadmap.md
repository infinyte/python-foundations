# Python Foundations — Interactive Learning Platform
### Build Summary & Iteration Roadmap

---

## What We Built

**Python Foundations** is a fully self-contained, browser-based interactive Python learning platform designed for a Senior Technical Product Owner with no current programming experience. It runs entirely from a single HTML file — no server, no install, no dependencies beyond a browser and an internet connection.

---

## Learner Profile

> **This section must be respected in every current and future lesson iteration.**

| Attribute | Detail |
|---|---|
| **Role** | Senior Technical Product Owner |
| **Domain (current)** | Private aviation — Signature Aviation (FBOs, ground handling, fuel operations, trip sheets, flight ops, aircraft services) |
| **Domain (prior)** | The Weather Channel — .NET developer (meteorological data, forecasting systems, weather APIs, alerts) |
| **Programming background** | Legacy .NET/C# experience (lapsed), no current Python experience |
| **Learning goal** | Gain enough Python fluency to automate tasks, read engineering code, and collaborate more precisely with dev teams |

### Domain Rotation Rule

To keep examples fresh and contextually relevant, **lessons must alternate between the two domains** in every iteration:

- **Odd-numbered lessons** → Private aviation domain (Signature Aviation context)
- **Even-numbered lessons** → Weather / meteorology domain (Weather Channel context)

This means a learner who works daily with FBO operations and once wrote C# weather alert parsers will immediately recognize the data, making abstract concepts concrete.

### Domain Vocabulary Reference

**Private Aviation (Signature)**

| Term | Meaning |
|---|---|
| FBO | Fixed Base Operator — the ground services facility at an airport |
| Trip sheet | Flight order document: tail number, routing, PAX count, services required |
| Fuel uplift | Volume of fuel loaded onto an aircraft (gallons or lbs) |
| FLIFO | Flight Information — real-time departure/arrival status |
| ICAO code | 4-letter airport identifier (e.g. KORL, KLAX, EGLL) |
| Tail number | Aircraft registration (e.g. N123AB) |
| Ground handling | De-icing, towing, catering, lavatory service, baggage |
| Ramp | The apron area where aircraft park and are serviced |

**Weather / Meteorology (Weather Channel)**

| Term | Meaning |
|---|---|
| METAR | Routine aviation weather observation report |
| TAF | Terminal Aerodrome Forecast — airport weather forecast |
| Dew point | Temperature at which air becomes saturated |
| Barometric pressure | Atmospheric pressure; key for weather pattern prediction |
| Watch / Warning / Advisory | NWS alert severity levels (ascending: Advisory → Watch → Warning) |
| UV index | Ultraviolet radiation intensity forecast |
| Wind shear | Sudden change in wind speed/direction — critical for aviation |
| Anomaly | Deviation from historical average (e.g. +8°F above seasonal norm) |

---

### Core Features

| Feature | Implementation |
|---|---|
| Python execution | Skulpt 1.2.0 (pure JS interpreter, no WASM) — **inlined directly in HTML** |
| Code editor | CodeMirror 5 with Dracula theme and Python syntax highlighting |
| Runtime loading | None — Skulpt is fully inlined; no CDN, no network request, no async boot |
| Keyboard shortcut | `Ctrl+Enter` / `Cmd+Enter` to run code |
| Progress tracking | Per-lesson completion state, progress bar in top bar |
| Quizzes | Multiple-choice with instant feedback after every lesson |
| Navigation | Sidebar + Previous/Next buttons |
| Design | Dark IDE aesthetic (GitHub-inspired), `JetBrains Mono` + `Syne` + `Lato` typography |

### Lesson Curriculum (v1.0)

Six lessons are included, each with explanatory content, a hands-on code challenge, and a knowledge-check quiz. v1.0 examples use a neutral **sprint/agile** framing as an accessible on-ramp. From **Iteration 2 onward**, all examples use the alternating private aviation / weather domain rotation defined in the Learner Profile above.

| # | Lesson | Key Concepts | Domain |
|---|---|---|---|
| 01 | Introduction to Python | `print()`, comments, arithmetic operators | Agile/neutral |
| 02 | Variables & Data Types | `str`, `int`, `float`, `bool`, f-strings, `type()` | Agile/neutral |
| 03 | Lists & Dictionaries | Indexing, `.append()`, `.remove()`, `len()`, `in`, key-value access | Agile/neutral |
| 04 | Control Flow | `if` / `elif` / `else`, comparison operators, `for` loops, `while` loops | Agile/neutral |
| 05 | Functions | `def`, parameters, return values, default parameters, docstrings | Agile/neutral |
| 06 | Working with Data | List comprehensions, `sorted()` + `lambda`, `sum()`, aggregation, string methods | Agile/neutral |

### Technical Architecture

```
python_lessons_final.html  (~989 KB, fully self-contained)
│
├── <head>
│   ├── CodeMirror CSS (cdnjs)
│   ├── Google Fonts (JetBrains Mono, Syne, Lato)
│   ├── <script> Skulpt 1.2.0 core    — inlined (~559 KB)
│   └── <script> Skulpt 1.2.0 stdlib  — inlined (~407 KB)
│
├── <body>
│   ├── #loadingOverlay      — shown briefly during initSkulpt()
│   ├── .topbar              — branding + progress bar + status badge
│   ├── .sidebar             — lesson navigation
│   ├── .lesson-panel        — scrollable lesson content (injected per lesson)
│   └── .editor-panel
│       ├── CodeMirror editor
│       └── Output console
│
└── <script>  (our application code)
    ├── LESSONS[]            — lesson data array (badge, title, html, code)
    ├── initSkulpt()         — synchronous Sk.configure() → enable Run button
    ├── runCode()            — Sk.misceval.asyncToPromise() execution + output
    ├── renderLesson()       — injects lesson HTML + sets editor starter code
    ├── checkAnswer()        — quiz interaction handler
    └── updateProgress()     — completion tracking
```

### Key Engineering Decisions

**Why Skulpt instead of Pyodide?**
Pyodide relies on WebAssembly, which is blocked in sandboxed iframe environments (including the Claude artifact sandbox). Skulpt is a Python 3 interpreter implemented entirely in JavaScript — no WASM, no large binary download, no sandbox restrictions. The tradeoff is that Skulpt does not support third-party packages (`pandas`, `numpy`, etc.), but it fully covers everything taught in this curriculum.

**Why inline Skulpt instead of loading from a CDN?**
Three CDN sources were attempted during development (cdnjs, skulpt.org, unpkg) and all failed in different contexts: Skulpt 1.2.0 did not exist at the cdnjs URL, skulpt.org was unreachable, and `fetch()`-based approaches were blocked by CORS from `file://` origins. The definitive fix was to download the Skulpt 1.2.0 core and stdlib at build time and embed them directly in the HTML. The result is a ~989 KB file that requires no network access whatsoever after the initial download.

**Why a single HTML file?**
Portability. The file can be opened directly in any browser, shared via email, hosted on any static file server, or embedded in a Confluence page — no build step, no framework, no deployment pipeline required.

---

## Iteration Roadmap

The platform is designed as a data-driven lesson array (`LESSONS[]`). Adding a new lesson requires only adding one object to that array — no changes to the UI, routing, or rendering logic are needed.

### How to Add a Lesson

Each lesson is a JavaScript object with four fields:

```javascript
{
  badge:    "Lesson 07 · Error Handling",   // shown above the title
  title:    "Error Handling & Debugging",   // large heading
  subtitle: "One-line description...",      // subheading paragraph
  html:     `...lesson HTML content...`,   // full lesson body (supports concept cards,
                                            // data grids, challenge boxes, quiz)
  code:     `# Starter code for editor\n...`
}
```

Push the object into the `LESSONS` array and update the sidebar HTML with a matching `id="nav-N"` entry. That's it.

---

### Planned Lesson Iterations

> **Domain rotation applies from Lesson 07 onward.** Odd = Private Aviation · Even = Weather/Meteorology.

---

#### Iteration 2 — Error Handling & File I/O
**Target audience:** PO who wants to understand why code breaks and how engineers handle edge cases.

| Lesson | Concepts | Domain | Example Context |
|---|---|---|---|
| 07 · Error Handling | `try` / `except` / `finally`, `raise`, `ValueError`, `TypeError`, `KeyError` | ✈️ Private Aviation | Parsing a trip sheet where the tail number field is missing or malformed |
| 08 · String Deep Dive | Slicing, `.join()`, `.split()`, `.replace()`, `.strip()`, `.startswith()` | 🌩️ Weather | Parsing a raw METAR string (`"KORL 281553Z 18008KT 10SM FEW045 28/17 A2992"`) into its component fields |
| 09 · Working with Files | Reading/writing `.txt` and `.csv`, `with open(...)` context manager | ✈️ Private Aviation | Reading a CSV of daily fuel uplifts by tail number; writing a summary report |

**PO framing:** "Why did the import fail?" — understanding errors makes sprint conversations with engineers more productive. The trip sheet parser mirrors real work the learner has seen in FLIFO/flight ops systems.

---

#### Iteration 3 — Object-Oriented Python
**Target audience:** PO who wants to understand how engineers model the domain — how real-world things become code.

| Lesson | Concepts | Domain | Example Context |
|---|---|---|---|
| 10 · Classes & Objects | `class`, `__init__`, instance variables, instance methods | 🌩️ Weather | A `WeatherStation` class with properties for location, temperature, wind speed, and a method to generate a METAR-style summary |
| 11 · Inheritance | Subclasses, method overriding, `super()` | ✈️ Private Aviation | A base `Aircraft` class, subclassed into `TurbopropAircraft` and `JetAircraft` with type-specific fuel burn calculations |
| 12 · Real-World OOP | Composition, `__str__`, `__repr__`, working with lists of objects | 🌩️ Weather | A `WeatherAlert` class and an `AlertManager` that holds a list of active Watch/Warning/Advisory objects and filters by severity |

**PO framing:** Every domain entity your engineers argue about in refinement — `Aircraft`, `TripSheet`, `WeatherStation`, `FuelOrder` — is a class in the codebase. Understanding this closes the gap between requirements and implementation.

---

#### Iteration 4 — Data & Automation
**Target audience:** PO who wants to automate something real and tangible at work.

| Lesson | Concepts | Domain | Example Context |
|---|---|---|---|
| 13 · CSV & Data Processing | `csv` module, reading rows, filtering, aggregating, writing output | ✈️ Private Aviation | Read a month of fuel uplift records → calculate total gallons per tail number → write a cost summary CSV |
| 14 · Working with JSON | `json` module, `json.loads()`, `json.dumps()`, nested access | 🌩️ Weather | Parse a mock weather API JSON response (temp, wind, alerts) and extract the fields relevant to a Go/No-Go flight decision |
| 15 · Simulated REST APIs | `urllib.request`, response parsing, headers, status codes (simulated) | ✈️ Private Aviation | Query a mock FLIFO endpoint to retrieve flight status for a tail number; handle 200, 404, and 503 responses gracefully |
| 16 · Capstone Project | End-to-end data pipeline combining all skills | 🌩️ Weather | Read a CSV of historical weather observations → detect days where conditions exceeded thresholds → generate a formatted anomaly report |

**PO framing:** The Capstone in Lesson 16 produces a real deliverable from raw data — the kind of ad-hoc analysis that currently requires requesting dev time or wrestling with Excel pivot tables.

---

#### Iteration 5 — Platform Enhancements
These are improvements to the platform itself, independent of lesson content. Implement these as needed to improve the learning experience.

| Enhancement | Description | Priority |
|---|---|---|
| **Hint system** | Collapsible "Show Hint" button per lesson that reveals a partial solution | High |
| **Expected output panel** | Show expected output alongside actual so learners can self-check without guessing | High |
| **localStorage persistence** | Save completed lessons so progress survives a page refresh | Medium |
| **Multiple code tabs** | Allow a lesson to have 2–3 editor tabs (e.g., `main.py` + `data.csv` mockup) | Medium |
| **Lesson locking** | Require completing a lesson before unlocking the next (optional/configurable) | Low |
| **Mobile layout** | Responsive breakpoint: stack editor below lesson panel on narrow screens | Medium |
| **Dark/light theme toggle** | Accessibility option | Low |
| **Export progress report** | Printable summary of completed lessons, quiz scores, and time spent | Low |

---

## Contribution Guide (for future AI-assisted iterations)

When asking an AI assistant to add a new lesson, provide the following context block in full. It contains everything needed to generate a correctly structured, domain-appropriate lesson without any back-and-forth.

```
Platform: python_lessons_final.html (single HTML file, Skulpt interpreter inlined)
Learner:  Senior Technical Product Owner
          - Current domain: Private aviation (Signature Aviation — FBOs, fuel ops,
            trip sheets, tail numbers, FLIFO, ICAO codes, ramp/ground handling)
          - Prior domain: The Weather Channel — .NET developer
            (METAR, TAF, weather alerts, temp/wind/pressure data, anomaly detection)
          - Background: Lapsed .NET/C#, no current Python experience

Domain rotation rule:
  - Odd-numbered lessons  → Private aviation context
  - Even-numbered lessons → Weather / meteorology context

Lesson array: LESSONS[] in the <script> block
Lesson object shape:
  {
    badge:    "Lesson NN · Section Name",
    title:    "Lesson Title",
    subtitle: "One-sentence framing for the learner",
    html:     `...lesson body HTML...`,
    code:     `# Python starter code for the editor`
  }

Available HTML components (use these for consistent styling):
  .concept-card              — bordered callout box with left accent stripe
  .concept-card.tip          — green stripe (use for best practices, pro tips)
  .concept-card.warning      — yellow stripe (use for gotchas, common mistakes)
  .concept-card.example      — purple stripe (use for code examples)
  .data-type-grid            — 2-column card grid (good for comparing types/options)
  .challenge-box             — purple-tinted challenge prompt at end of lesson
  .quiz-container            — wraps the quiz section
  .quiz-option               — each answer choice (clickable div)
  .quiz-feedback             — feedback message shown after answer selected
  checkAnswer(el, bool, msg) — quiz handler: el=clicked element, bool=correct?, msg=feedback text

Sidebar entry to add (replace N with lesson number, 0-indexed):
  <div class="lesson-item" id="nav-N" onclick="goToLesson(N)">
    <div class="lesson-dot"></div>Lesson Title
  </div>

Sidebar section headers (add lesson under correct group or create new group):
  "Fundamentals"       — Lessons 01–03
  "Logic & Structure"  — Lessons 04–05
  "Real-World Python"  — Lessons 06+
```

### Quality Checklist for Each New Lesson

Before submitting a new lesson for inclusion, verify:

- [ ] Domain matches the rotation rule (odd = aviation, even = weather)
- [ ] All variable names, data structures, and print output use domain-relevant vocabulary
- [ ] Starter code runs without errors in Skulpt (test with `print`, basic loops, dicts — no third-party imports)
- [ ] Concept cards are used consistently (tip/warning/example)
- [ ] A `.challenge-box` section is included with 3–4 specific tasks
- [ ] The quiz has exactly 3 options, one correct, with meaningful feedback for wrong answers
- [ ] No `.NET`/C# comparisons are made that would confuse rather than illuminate
- [ ] The sidebar entry is added with the correct 0-based index

---

## File Inventory

| File | Purpose |
|---|---|
| `python_lessons_final.html` | The complete interactive learning platform (~989 KB, fully self-contained) |
| `python_foundations_roadmap.md` | This document — build summary and iteration plan |

---

*Last updated: March 2026 · Built with Claude, CodeMirror, and Skulpt (inlined) · Designed for a Senior Technical Product Owner in private aviation*
