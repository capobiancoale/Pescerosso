# Fiorebianco — Thesis repository setup

This file explains how to organise the repository `github.com/alex542323/Fiorebianco` or 'github.com/capobiancoale/Pescerosso' for the master's thesis, and how to push the drafts produced in the chat session to GitHub.

---

## Suggested repository structure

```
Fiorebianco/
├── README.md                       ← project overview (write this later)
├── progress.md                     ← the thesis progress tracker
├── 00-brief/
│     ├── thesis-standing-brief.docx    (from earlier chat)
│     ├── cowork-playbook.docx          (from earlier chat, optional)
│     └── official-guidelines.pdf       (from Master's office)
├── 01-source-materials/            ← your notes, screenshots, technical docs
│     └── (empty for now, populate over time)
├── 02-references/
│     ├── references-bib.md         ← running bibliography
│     └── papers/                   ← PDFs of key sources (optional)
├── 03-drafts/                      ← WORK IN PROGRESS
│     ├── section-04-problem-definition.md
│     ├── section-05-1-overall-approach.md
│     ├── section-05-2-data-architecture.md
│     └── section-05-3-semantic-model.md
├── 04-feedback/                    ← your written feedback per section
└── 05-final/                       ← consolidated Word doc at the end
```

---

## How to push these drafts to your GitHub repo

I do not have write access to GitHub from this chat, so you push the files yourself. The workflow is straightforward.

### Option A: from the terminal (recommended if you have git installed)

```bash
# 1. Clone the repo (only once)
git clone https://github.com/alex542323/Fiorebianco.git
cd Fiorebianco

# 2. Create the folder structure
mkdir -p 00-brief 01-source-materials 02-references 03-drafts 04-feedback 05-final

# 3. Download the files from the chat and move them into place
#    (put section-*.md into 03-drafts/, references-bib.md into 02-references/,
#     progress.md and README-repo-setup.md at the root)

# 4. Commit and push
git add .
git commit -m "Add thesis sections 4 and 5.1-5.3, references, progress tracker"
git push origin main
```

### Option B: via the GitHub web interface

1. Go to https://github.com/alex542323/Fiorebianco
2. For each folder listed above: click "Add file → Create new file", type the folder path followed by the filename (e.g. `03-drafts/section-04-problem-definition.md`), then paste the content. GitHub creates the folder automatically.
3. For file uploads: click "Add file → Upload files", drag the downloaded `.md` files, commit at the bottom.

### Option C: Claude Desktop with Claude Code

If you install Claude Desktop and use Claude Code (with git integration), you can delegate the push to it directly. The workflow is: clone locally once, then in each chat session ask Claude Code to write new sections directly into the local repo folder and commit them for you.

---

## Every time we write a new section together

The workflow to keep the repo in sync:

1. In the chat, we produce a new section draft (or revise an existing one).
2. I save the file locally in the chat sandbox and present it for download.
3. You download the file and place it in `03-drafts/` (or overwrite the existing version if it's a revision — I number them v1, v2, v3 in the filename to avoid losing history).
4. `git add` + `git commit` + `git push`.
5. Update `progress.md` if the section is a new one.

Small tip: commit often, with clear commit messages ("Section 5.4 KPI design draft v1", "Section 4 revision v2 after feedback"). Small commits are easier to review and easier to roll back if a revision goes badly.
