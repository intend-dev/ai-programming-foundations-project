# AI Programming Foundations Project: Titanic Data Workflow

**Author:** Francisco Mateus

## Project Description

A reproducible data workflow over the Titanic passenger manifest, built for the
AI Programming Foundations capstone. The notebook measures what is missing,
cleans the table with four pure functions and an EDA helper, explores survival
by sex, class, title and deck, draws five labelled figures, and closes with a
summary that separates what the numbers support from what they cannot say.

## What I Built

- `data_workflow.ipynb`: the executed notebook, with Setup, Ingestion,
  Assessment, Cleaning, Exploratory Analysis, Visualizations and Summary
  sections.
- Four pure cleaning functions, each with a docstring (title extraction, the
  `177` age fills by title median, deck extraction, the port fill): each copies
  the frame, so the loaded table is never changed in place, and the cleaning
  cell asserts every gap closed.
- One EDA function (`rate_table`): survival count, group size and rate per
  group.
- Five labelled figures, each with a title, labeled axes, and a written
  interpretation.
- `requirements.txt`: the pinned environment, captured with `pip freeze`.
- `module_summary.md`: the written module summary, with the five figures
  embedded and every number traced to a printed notebook output.
- `data_workflow.html` and `data_workflow.pdf`: exported snapshots of the
  executed notebook, readable without any Jupyter setup.

## Key Findings

| Group | Survival rate |
|---|---:|
| Overall | `38.38%` |
| Women | `74.20%` |
| Men | `18.89%` |
| First class | `62.96%` |
| Second class | `47.28%` |
| Third class | `24.24%` |

These are associations from one voyage, not causes. The title breakdown
(`Mrs` `79.20%` against `Mr` `15.67%`) and the cabin-deck rates, with their
caveats, are in `module_summary.md`.

## Files

| File | What it is |
|---|---|
| `data_workflow.ipynb` | the executed notebook, run top to bottom |
| `titanic.csv` | the dataset, read from disk |
| `figures/` | the five figure PNGs, saved by the notebook's figure cells on each run |
| `requirements.txt` | the pinned environment (a full `pip freeze`) |
| `module_summary.md` | the written module summary (the report) |
| `data_workflow.html` | HTML snapshot of the executed notebook |
| `data_workflow.pdf` | PDF snapshot of the executed notebook |
| `README.md` | this file |

## Dataset

Titanic: Machine Learning from Disaster (Kaggle):
<https://www.kaggle.com/c/titanic/data>

`titanic.csv` ships with the project and sits beside the notebook, so a run
needs no internet connection. The file mirrors
`https://raw.githubusercontent.com/datasciencedojo/datasets/master/titanic.csv`
and holds `891` rows and `12` columns.

## Tech Stack

Python 3.13.15 with numpy 2.5.3, pandas 3.0.6, matplotlib 3.11.2 and seaborn
0.13.2 (all pinned), run through Jupyter.

## How to Run

```text
uv venv --python 3.13 .venv
uv pip install --python .venv -r requirements.txt
.venv\Scripts\python.exe -m nbconvert --to notebook --execute --inplace data_workflow.ipynb
```

### Alternative Setup

You can also use Python's built-in virtual environment tools instead of `uv`.
Create a virtual environment, activate it, and install the required packages
using `requirements.txt`. Activation differs by shell:

- PowerShell: `.venv\Scripts\Activate.ps1`
- Command Prompt: `.venv\Scripts\activate.bat`
- macOS and Linux: `source .venv/bin/activate`

The dependency file was generated on Windows and carries two Windows-only
packages (`colorama`, `pywinpty`); on macOS or Linux, install `numpy pandas
matplotlib seaborn jupyter nbconvert` at the pinned versions instead.

You can also open `data_workflow.ipynb` in Jupyter and run the cells manually.
If you execute it using the command line, the notebook's saved outputs will
be overwritten, so back up the notebook first if you want to keep the stored
outputs.

### Updating Requirements

To update `requirements.txt` from the project's `.venv`:

```text
uv pip freeze --python .venv | Out-File requirements.txt -Encoding ascii
```

The `Out-File` step sidesteps a PowerShell quirk where `>` writes UTF-16; on
macOS and Linux, standard output redirection (`>`) is sufficient.

The generated file may differ from the committed version: it lists `pip`
itself and spells some names with hyphens (`jupyter-client` for the committed
`jupyter_client`), which `pip` resolves identically.

Make sure the freeze runs in the project's environment; a bare `python` on
`PATH` captures the system interpreter instead of this `3.13.15` venv.
Remember that `pip freeze` lists all installed packages, not just the ones
the notebook uses.

## Bias Awareness

- **Missing ages are not random.** `177` passengers (`19.87%`) have no recorded
  age, so dropping those rows would tilt survival toward the groups that
  recorded more. Each blank takes its title group's median, an estimate rather
  than an observation.
- **`Cabin` is `77.10%` missing.** Only `204` of `891` passengers have a cabin
  record; dropping the blanks would remove most of the ship, so they stay as
  the `U` deck category, and the deck readings rest on the `204`.
- **Zero fares stayed.** `15` rows carry a `0` fare and look like real records;
  deleting unusual values to tidy a table is itself a bias decision, so they
  are kept as they are.
- **One voyage.** Every number describes one voyage of one ship; nothing
  generalises beyond this record, and every finding is an association, not a
  cause.

## Future Integration Reflections

- **ML workflow.** Add a train/test split, engineer features (family size from
  `SibSp` and `Parch`, fare per person, age bands), train classifiers, report
  accuracy, precision, recall, F1 and ROC-AUC, and read feature importance. The
  pure functions make that split safe to reuse, because the same frame can be
  transformed twice without aliasing.
- **Neural-network preparation.** Normalise `Age` and `Fare`, one-hot encode
  `Sex`, `Embarked`, `deck` and `title`, define `Survived` as the target,
  convert to tensors, and fix the random seeds.
- **Agentic automation.** An agent could profile a new table, propose a fill
  strategy from the missingness pattern, flag anomalies, run the notebook end
  to end, and check `requirements.txt` against the environment before drafting
  the report from the stored outputs.

## Repository

The project is version controlled, committed section by section on
`development`, alongside `main`. The repository link is supplied with the
submission.