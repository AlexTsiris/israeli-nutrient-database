# The Israeli National Nutrient Database

Course project for **Advanced Topics in Machine Learning / Data Mining (2026)**, Ariel University.

This project uses the Israeli National Nutrient Database from the Ministry of Health.
The goal is to build a full classification pipeline and an unsupervised analysis on this data.

> **Team:** _TODO: add the full names of all team members here._

---

## About the dataset

The data comes from the Israeli government open data portal:

- **Portal page:** https://data.gov.il/datasets/ministry-health/nutrition-database
- **Publisher:** Ministry of Health (משרד הבריאות)
- **Main file:** `data/raw/moh_mitzrachim.csv` — **4,624 rows × 85 columns**

Every row is a food item or a cooked dish. Every column is a nutrient value.
All values are given **per 100 grams**, so all foods can be compared directly.

The database contains both raw foods (for example `Milk, cow, 3% fat`) and
prepared dishes (for example `Bulgur, cooked with white beans and tomatoes`).

## Files in `data/raw/`

| File | Size | What it contains |
|---|---|---|
| `moh_mitzrachim.csv` | 4,624 × 85 | **Main file.** Foods and dishes with 85 nutrient columns per 100 g |
| `moh_matkonim.csv` | 8,324 × 8 | Recipes: which ingredients are used in each dish, and how much |
| `moh_yehidot_mida.csv` | 108 × 2 | List of measurement units (code → name) |
| `moh_yehidot_mida_lemitzrachim.csv` | 19,024 × 3 | Weight in grams for one unit of each food |

### How the files connect

The main file uses the column `smlmitzrach` as the food code.
The other two files use the name `mmitzrach` for the same code.
The column names are different, but the values are the same, so you can join on them.

### The class label

The dataset has no ready "food category" column. The category is **inside the food code**.
The column `smlmitzrach` is hierarchical:

- the **first digit** gives 9 main food groups
- the **first two digits** give 50 smaller sub-groups

For example, `11111009` (*Milk, cow, 3% fat*) starts with `11`, which is the dairy group.

The 9 main groups are:

| Digit | Group | Rows |
|---|---|---|
| 1 | Milk and dairy | 527 |
| 2 | Meat, poultry and fish | 915 |
| 3 | Eggs | 45 |
| 4 | Legumes and nuts | 272 |
| 5 | Grains and baked goods | 1,064 |
| 6 | Fruit | 299 |
| 7 | Vegetables | 882 |
| 8 | Fats and oils | 87 |
| 9 | Drinks and other | 533 |

The classes are **not balanced**. The smallest group has 45 rows and the largest has 1,064.
At the 50-class level the imbalance is much stronger. This makes the data a good example
for methods that deal with imbalanced data.

## How the data was downloaded

The download links on the portal are behind a Google login, so they do not work for scripts.
The open CKAN API was used instead. It needs no login:

```
https://data.gov.il/api/3/action/datastore_search?resource_id=<RESOURCE_ID>&limit=1000&offset=<N>
```

The API returns 1,000 rows per request, so the script asks for pages until all rows are read.
The resource id of the main file is `c3cb0630-0650-46c1-a068-82d575c094b2`.

All files are saved in `utf-8-sig` encoding, so Hebrew text opens correctly in Excel.

## Repository structure

```
.
├── data/
│   └── raw/        # original data from data.gov.il (not changed)
├── code/           # scripts and notebooks
├── figures/        # plots for the report
├── outputs/        # results, tables, model metrics
├── docs/           # proposal and final write-up
└── requirements.txt
```

## Planned work

**Part 1 — Classification pipeline.** Predict the food group from the nutrient values.
We will train and compare at least 5 models, and we will test methods for imbalanced data.

**Part 2 — Unsupervised analysis.** We will compare at least 3 clustering models on the same
data. Then we will check how close the clusters are to the real food groups.

**Optional.** The recipe file makes it possible to build a simple recommendation system,
for example to suggest a replacement for an ingredient with a similar nutrient profile.

## Setup

```bash
pip install -r requirements.txt
```

## Note on generative AI

Following the course rules, any code or text created or edited with the help of a
generative AI tool is marked with a comment in the code, or in red in the report.

## Data license

The data is public open data from https://data.gov.il/ and belongs to the Israeli
Ministry of Health. It is included here so that the project can be reproduced.
