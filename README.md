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

## Files

| File | What it is |
|---|---|
| `data_workflow.ipynb` | the executed notebook, run top to bottom |
| `titanic.csv` | the dataset, read from disk |
| `figures/` | the five figure PNGs, saved by the notebook's figure cells on each run |
| `requirements.txt` | the pinned environment (a full `pip freeze`) |
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

Standard-library alternative: `python -m venv .venv`, then activate in PowerShell
with `.venv\Scripts\Activate.ps1` or in Command Prompt with
`.venv\Scripts\activate.bat` (macOS and Linux: `source .venv/bin/activate`),
then `pip install -r requirements.txt`. The freeze was captured on
Windows and includes two Windows-only packages (`colorama`, `pywinpty`); on
macOS or Linux install `numpy pandas matplotlib seaborn jupyter nbconvert` at
the pinned versions instead. Opening `data_workflow.ipynb` and running all
cells does the same thing; the nbconvert command rewrites the notebook in
place, so copy it first if you want to keep the stored outputs.

To regenerate the environment file from your own environment:

```text
pip freeze > requirements.txt
```

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