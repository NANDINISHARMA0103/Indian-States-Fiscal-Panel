# Does State Fiscal Behaviour Shape Income?
### Panel evidence from 31 Indian states and union territories, 2011-12 to 2021-22

> **In one line:** across Indian states, **higher revenue expenditure goes with *lower* per capita income**, while social-sector spending, capital spending, fiscal deficits and debt show **no reliable link** once state differences are controlled for.

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Panel econometrics](https://img.shields.io/badge/Panel%20data-FE%20%7C%20RE%20%7C%20Pooled%20OLS-1F6FEB)
![Plotly](https://img.shields.io/badge/Plotly-animated%20charts-3F4F75?logo=plotly&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green)

![Fixed-effects estimates](fe_coefficients.png)

*Effect of +1 percentage point of GSDP on per capita NSDP (%), fixed-effects specification B, 95% confidence intervals.*

---

## The question

Indian states differ widely in how they spend and borrow. Some run large deficits and carry heavy debt, others stay lean. This project asks whether those fiscal choices are linked to how rich a state's people are, measured by **per capita Net State Domestic Product (NSDP)**.

It tests five hypotheses, each about one fiscal indicator measured as a share of GSDP:

| | Fiscal indicator | Hypothesis | Test | Coefficient | p-value | Verdict |
|---|---|---|---|---|---|---|
| H1 | Social-sector expenditure | Raises per capita NSDP | one-sided | +0.037 | 0.468 | Not supported |
| H2 | Capital expenditure | Raises per capita NSDP | one-sided | +0.097 | 0.410 | Not supported |
| H3 | Revenue expenditure | Is significantly related | two-sided | **−0.884** | **0.020** | **Supported** (negative) |
| H4 | Fiscal deficit | Is significantly related | two-sided | +0.246 | 0.464 | Not supported |
| H5 | Outstanding liabilities | Is significantly related | two-sided | −0.201 | 0.231 | Not supported |

*Fixed-effects results, spec B. A coefficient of −0.884 means per capita NSDP is about 0.88% lower for each extra percentage point of GSDP spent on revenue expenditure. Source: `results/Task6_Tables.xlsx`.*

## What the data show

<table>
<tr>
<td width="50%"><img src="fiscal_trends.png" alt="Fiscal deficit and liabilities over time"></td>
<td width="50%"><img src="state_rankings.png" alt="State rankings"></td>
</tr>
<tr>
<td><b>Rising burden.</b> Average fiscal deficit and outstanding liabilities both climbed over the decade, with a sharp jump in the COVID year 2020-21.</td>
<td><b>Big gaps between states.</b> Average deficit and debt ratios range widely, from near zero in Delhi to over 7% of GSDP (deficit) and over 60% (liabilities) in the highest states.</td>
</tr>
</table>

## How robust is it?

![Robustness checks](robustness_checks.png)

The revenue-expenditure result is the only one that holds up. Its estimate stays negative in all six robustness checks: winsorising at 1%/99%, dropping the seven north-eastern special-category states, dropping all eleven special-category states, adding a post-split dummy (Andhra Pradesh and Telangana), using lagged regressors, and excluding 2020-21. The lagged-regressor interval is borderline. The other four indicators stay statistically indistinguishable from zero.

## Method at a glance

| Step | What was done |
|---|---|
| **Data** | 31 states and UTs × 11 years. Fiscal deficit, revenue, capital and social-sector spending and outstanding liabilities as shares of GSDP, plus per capita NSDP and GSDP |
| **Cleaning** | Standardised state names and year labels, checked duplicates and missing values, recalculated ratios and cross-checked GSDP. Suspect outstanding-liabilities values (for example Delhi 2017-18 to 2020-21 and several 2014-15 entries) were set to missing |
| **Outcome** | log of per capita NSDP, with year effects in every model and state-clustered standard errors |
| **Models** | Pooled OLS, fixed effects (within) and random effects (Swamy-Arora), estimated with custom numpy/scipy code and the `linearmodels` package |
| **Specifications** | **A** includes log GSDP, **B** drops log GSDP, **C** also drops revenue expenditure |
| **Model choice** | F-test (FE vs pooled), Breusch-Pagan LM (RE vs pooled), Hausman and a robust Hausman/Mundlak test (FE vs RE) |
| **Robustness** | Six checks listed above |

| Test | A | B | C |
|---|---|---|---|
| F-test, FE vs pooled | reject pooled | reject pooled | reject pooled |
| Breusch-Pagan LM, RE vs pooled | reject pooled | reject pooled | reject pooled |
| Hausman (FE vs RE) | FE (p < 0.001) | RE (p = 0.159) | RE (p = 0.327) |

In spec B the fixed-effects and random-effects estimates are nearly identical, so the conclusions do not depend on that choice.

## Interactive charts

Seven animated Plotly charts are in [`animations/`](animations/). GitHub shows HTML files as code, so view them through **GitHub Pages** (see below) or download and open them in a browser.

| Chart | Shows |
|---|---|
| `anim_1_trend.html` | Fiscal indicators drawing year by year |
| `anim_2_bubble_social.html` | Social-sector spending vs income, animated by year |
| `anim_2_bubble_capital.html` | Capital spending vs income, animated by year |
| `anim_3_race_social.html` | Top 15 states by social-sector spending ratio, year by year |
| `anim_3_race_capital.html` | Top 15 states by capital spending ratio |
| `anim_3_race_fiscal_deficit.html` | Top 15 states by fiscal deficit ratio |
| `anim_3_race_liabilities.html` | Top 15 states by outstanding liabilities ratio |

## Repository structure

```
indian-states-fiscal-panel/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── Python_Assignment.ipynb          # cleaning, EDA, models, tests, robustness, charts
├── data/
│   └── States_Data_Cleaned.xlsx         # cleaned panel (341 state-years, 19 columns)
├── results/
│   ├── Tasks_3_to_5_Results.xlsx        # pooled / FE / RE by spec, model-choice tests, robustness
│   └── Task6_Tables.xlsx                # summary coefficient table and H1-H5 results
├── images/                              # charts used in this README
├── animations/                          # 7 interactive Plotly charts
└── reports/
    └── Indian_States_Fiscal_Final.pptx  # presentation
```

## Reproduce it

The notebook was built in **Google Colab**.

1. Open `notebooks/Python_Assignment.ipynb` in Colab.
2. Upload the input workbook when prompted. The cleaning section reads `Final States Data.xlsx` and writes `States_Data_Cleaned.xlsx`.
3. Run the cells in order: cleaning → descriptive statistics and charts → estimators → Tasks 3 to 5 (models, tests, robustness) → Task 6 (summary and hypotheses).

```bash
pip install -r requirements.txt
```

## Publish the animations (optional, 2 minutes)

1. In the repo, go to **Settings → Pages**.
2. Under *Build and deployment*, choose **Deploy from a branch**, select `main` and `/ (root)`, and save.
3. After a minute your charts are live at `https://<your-username>.github.io/indian-states-fiscal-panel/animations/anim_1_trend.html`, and the same pattern works for the others.

## Limitations

- **Association, not causation.** Fixed effects control for stable state traits, but spending and income can influence each other.
- **Short panel.** Eleven years, with the COVID shock of 2020-21 inside the window.
- **Data repairs.** Some liabilities values looked misaligned and were set to missing rather than corrected.
- **Pooled estimates differ sharply from within-state ones** (for example social-sector spending), which is why the within-state (fixed-effects) results are the ones reported.
- **Statistical power.** With 31 units, wide confidence intervals mean small effects cannot be ruled out for the non-significant indicators.

## Author

**Nandini Sharma**
MSc Economics (2025–27), Symbiosis School of Economics, Pune
GitHub: [@your-username](https://github.com/your-username)

## License

Code is released under the [MIT License](LICENSE). Data are compiled from public state-finance and national-accounts sources and are used here for educational purposes only.
