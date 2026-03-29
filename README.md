# 🐍 Python Foundations

> An interactive, browser-based Python learning platform for a Senior Technical Product Owner in private aviation.

**[▶ Launch the Tutorial](https://infinyte.github.io/python-foundations)**

---

## What This Is

Python Foundations is a fully self-contained, interactive coding tutorial delivered as a single HTML file. It runs entirely in the browser — no installation, no Python runtime, no server, no internet connection required after the initial page load.

The curriculum is designed for someone with a technical background (lapsed .NET/C#) who wants enough Python fluency to automate tasks, read engineering code, and collaborate more precisely with development teams.

---

## Curriculum — v1.0

| # | Lesson | Concepts |
|---|---|---|
| 01 | Introduction to Python | `print()`, comments, arithmetic operators |
| 02 | Variables & Data Types | `str`, `int`, `float`, `bool`, f-strings, `type()` |
| 03 | Lists & Dictionaries | Indexing, `.append()`, `len()`, `in`, key-value access |
| 04 | Control Flow | `if`/`elif`/`else`, comparison operators, `for`/`while` loops |
| 05 | Functions | `def`, parameters, return values, default parameters, docstrings |
| 06 | Working with Data | List comprehensions, `sorted()` + `lambda`, aggregation, string methods |

Future iterations will add Error Handling, OOP, CSV/JSON processing, and REST API basics — all using private aviation and meteorology domain examples. See [`docs/roadmap.md`](docs/roadmap.md) for the full plan.

---

## Tech Stack

| Component | Implementation |
|---|---|
| **Python interpreter** | [Skulpt 1.2.0](https://skulpt.org) — pure JavaScript, inlined directly in `index.html` |
| **Code editor** | [CodeMirror 5](https://codemirror.net) with Dracula theme and Python syntax highlighting |
| **Fonts** | JetBrains Mono · Syne · Lato (Google Fonts) |
| **Hosting** | GitHub Pages — static, no build step |

**Why Skulpt?** Skulpt is a Python 3 interpreter implemented entirely in JavaScript. Unlike Pyodide, it requires no WebAssembly, no SharedArrayBuffer, and no worker threads — making it compatible with sandboxed iframes, local `file://` URLs, and all static hosting environments.

**Why inlined?** CDN delivery of Skulpt was unreliable across testing environments. Inlining the ~967 KB library directly into `index.html` makes the file completely self-contained and eliminates all external runtime dependencies.

---

## Running Locally

```bash
# Just open it
open index.html
```

No build step. No `npm install`. No server needed.

---

## Adding Lessons

See [`docs/roadmap.md`](docs/roadmap.md) for the full contribution guide, domain vocabulary reference, and planned lesson iterations.

**Quick summary:** Each lesson is one JavaScript object in the `LESSONS[]` array inside `index.html`. Add the object, add a sidebar entry, done.

```javascript
{
  badge:    "Lesson 07 · Error Handling",
  title:    "Error Handling & Debugging",
  subtitle: "One-line framing for the learner.",
  html:     `...lesson body HTML...`,
  code:     `# Python starter code`
}
```

---

## Project Structure

```
python-foundations/
├── index.html          # Complete interactive tutorial (~989 KB, self-contained)
├── README.md           # This file
└── docs/
    └── roadmap.md      # Curriculum plan, learner profile, contribution guide
```

---

*Built with Claude · Powered by Skulpt & CodeMirror · Infinyte Software Solutions*
