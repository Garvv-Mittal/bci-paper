# 🧠 BCI Research Group — Literature Review Repository

> A collaborative repository for Brain-Computer Interface (BCI) research, maintained by two teams under the guidance of team leads **Garv Mittal** and **Virti Miglani**.

---

## 👥 Team

### Team Leads
| Name | GitHub |
|------|--------|
| Garv Mittal | [@Garvv-Mittal](https://github.com/Garvv-Mittal) |
| Virti Miglani | *(add GitHub handle)* |

### Members
| Name | Folder |
|------|--------|
| Aanya Gupta | `literature-review/Aanya/` |
| Ananya Kaushik | `literature-review/Ananya/` |
| Aumrika Jorder | `literature-review/Aumrika/` |
| Charvi Khandelwal | `literature-review/Charvi/` |
| Deepali Singh | `literature-review/Deepali/` |
| Gaurika | `literature-review/Gaurika/` |
| Hidanshu | `literature-review/Hidanshu/` |
| Priyanshu | `literature-review/Priyanshu/` |
| Riddhima Chawla | `literature-review/Riddhima/` |
| Sonvi Goyal | `literature-review/Sonvi/` |

---

## 📁 Repository Structure

```
bci-research/
├── README.md
└── literature-review/
    ├── Aanya/
    ├── Ananya/
    ├── Aumrika/
    ├── Charvi/
    ├── Deepali/
    ├── Gaurika/
    ├── Hidanshu/
    ├── Priyanshu/
    ├── Riddhima/
    └── Sonvi/
```

Each member folder contains that member's paper notes, summaries, and pre-reads organized by week or paper.

---

## 📖 Research Workflow

This repository tracks progress across six phases:

| Phase | Description |
|-------|-------------|
| **1. Literature Review** | Read and summarize assigned papers; upload notes to your folder |
| **2. Dataset Understanding** | Explore, document, and analyze the target BCI dataset |
| **3. Baseline Model** | Implement and document a baseline model from the literature |
| **4. Improvement** | Propose and implement improvements over the baseline |
| **5. Evaluation** | Evaluate results with standard BCI metrics |
| **6. Research Writing** | Compile findings into a structured report or paper draft |

---

## 📋 Weekly Protocol

Every week, each member is expected to:

1. **Read** the assigned paper(s) thoroughly.
2. **Create notes** in your personal folder using the standard template (see below).
3. **Commit and push** your notes before the weekly deadline.
4. **Open a Pull Request** targeting `main` — do **not** push directly to `main`.
5. Await review and merge by a team lead.

---

## 📝 Paper Notes Template

Create a new `.md` file in your folder for each paper. Use the following format:

```markdown
## <Paper Title>

---

## Paper Info

| Field | Details |
|-------|---------|
| **Title** | Full title of the paper |
| **Authors** | Author names |
| **Year** | Publication year |
| **Journal** | Journal or conference name |
| **Volume/Article** | Volume, issue, or article number |
| **DOI** | [doi-link](https://doi.org/...) |

---

## Summary

<!-- 3–5 sentence summary of what the paper is about -->

---

## Key Contributions

- Contribution 1
- Contribution 2
- Contribution 3

---

## Methodology

<!-- Describe the approach, model, or algorithm used -->

---

## Results

<!-- Key results, metrics, benchmarks -->

---

## Relevance to Our Project

<!-- How does this paper connect to our BCI research goals? -->

---

## Questions / Open Points

<!-- Anything you didn't understand or want to discuss -->
```

---

## 🔀 Git Rules (Strict)

> These rules keep the commit history clean and contributions attributable.

### Branching
- Each member works on their **own branch**, named after them (e.g., `riddhima`, `ananya`).
- **Never commit directly to `main`.**
- Branch off from `main` when starting new work.

### Commits
- Write clear, descriptive commit messages.
- Good: `Add EEGNet paper notes to Riddhima/`
- Bad: `update`, `stuff`, `notes`
- One logical change per commit — don't bundle unrelated updates.

### Pull Requests
- Open a PR from your branch into `main` when your weekly work is ready.
- Add a short description of what you added or changed.
- Wait for a team lead to review and merge — do **not** merge your own PR.

### Conflicts
- If you face merge conflicts, reach out to a team lead before force-pushing anything.

---

## 🚀 Getting Started

```bash
# 1. Clone the repo
git clone https://github.com/Garvv-Mittal/<repo-name>.git
cd <repo-name>

# 2. Switch to your branch (create if it doesn't exist)
git checkout -b <your-name>
# or, if your branch already exists:
git checkout <your-name>

# 3. Add your notes inside your folder
# e.g., literature-review/Riddhima/paper1_eegnet.md

# 4. Stage and commit
git add .
git commit -m "Add <paper-name> notes to <YourName>/"

# 5. Push your branch
git push origin <your-name>

# 6. Open a Pull Request on GitHub → base: main ← compare: <your-name>
```

---

## 📌 Compulsory Papers

The following papers are assigned to **all members** and must be read and summarized:

| # | Paper | Key Topic |
|---|-------|-----------|
| 1 | EEGNet: A Compact CNN for EEG-Based BCIs | EEG signal processing, CNNs |
| 2 | Tumor Diagnosis *(TBD)* | Medical BCI applications |
| 3 | FBCNet | Filter Bank CNNs for EEG |
| 4 | Entropy Model *(TBD)* | Signal entropy in BCIs |

> Notes for compulsory papers go in the `Paper/Compulsory Papers/` folder using the filename convention: `paper<N>_<shortname>.md`

---

## 🤝 Contributing

All contributions go through Pull Requests. If you have suggestions for improving this README or the folder structure, open a PR with your proposed changes and tag a team lead for review.

---

*Last updated: June 2026* 
