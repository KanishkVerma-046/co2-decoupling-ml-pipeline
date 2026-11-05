# Private notes for the author (NOT for submission)

## 1. Items needing your attention first

1. **The headline result is modest, and the pre-specified model is not the best one on the test set.** The simplicity rule chose the elastic-net logistic regression (CV PR-AUC 0.346 against the random forest's 0.384, within one standard error). On the single test run it beat the persistence rule (PR-AUC 0.390 against 0.284, paired difference +0.106, interval +0.051 to +0.192) but it is statistically no better than a trivial "income-group base rate" reference (0.379), and the random forest and HGB scored about 0.47. I did **not** switch models after seeing this; the report says so (Section 8.5) and labels the extra comparisons as post hoc (`results/test_posthoc.csv`). Decide whether you are comfortable presenting it this way. I think it is the stronger story, because it shows discipline and understanding, but it is not a flattering headline.
2. **At the frozen threshold (0.124) the logistic regression flags almost every test row** (recall 0.993, precision 0.254, accuracy 0.273, MCC 0.072). This is a known weakness of the F1-maximising rule under weak signal. The report presents it as a finding.
3. **Word count.** The build script counts 5,739 words of body prose (table captions are included; tables, code, headings, figure captions, references and appendices are excluded). A stricter count of prose lines only gives about 5,460. If your counter also includes headings and every caption, the total is about 6,460, which exceeds 6,000. If that matters, shorten captions or trim Section 8.5 or Section 6.
4. **scikit-learn is not cited.** Pedregosa et al. (2011, JMLR) has no Crossref record (JMLR issues no DOI), so under the "verify through Crossref or drop" rule it was dropped. The report names scikit-learn and its version in Section 7.5 without a citation. Add it by hand if your instructors expect it.
5. **Four reference DOI links return HTTP 403 to a script** (two ACM, two Wiley), which is publisher bot-blocking, not a missing record. Their metadata (title, journal, year, volume, pages, DOI) was verified through the Crossref API (`results/reference_check.csv`).
6. **Figure paths.** The Markdown uses `../figures/fig_*.png` as you asked. The converter must read the PNGs from the `figures/` folder at the repository root. All 14 figures are landscape or squarish.
7. **Wide tables.** Tables 8.8, 8.14 and 8.16 have 7 to 8 columns; set them to landscape or a smaller font in Word if they wrap badly. Table 9.1 has long cells.
8. **Not pushed.** Commands are in the final message of the session and below in section 7 of these notes.

## 2. Figure-to-section map

| Figure | File | Section |
|---|---|---|
| 1.1 Pipeline flowchart | `fig_pipeline_flowchart.png` | 1.4 |
| 2.1 Label construction and split timeline | `fig_label_split_timeline.png` | 2.3 |
| 2.2 Label base rate by year | `fig_label_base_rate.png` | 2.4 |
| 3.1 Missingness heatmap | `fig_missingness_heatmap.png` | 3.2 |
| 4.1 Feature correlation heatmap | `fig_feature_correlation.png` | 4.4 |
| 8.1 Imputation comparison | `fig_imputation_comparison.png` | 8.2 |
| 8.2 CV model comparison | `fig_cv_model_comparison.png` | 8.3 |
| 8.3 Test ROC curves | `fig_test_roc.png` | 8.4 |
| 8.4 Test precision-recall curves | `fig_test_precision_recall.png` | 8.4 |
| 8.5 Test confusion matrices | `fig_test_confusion_matrices.png` | 8.4 |
| 8.6 Test calibration | `fig_test_calibration.png` | 8.4 |
| 8.7 Permutation importance and coefficients | `fig_importance_coefficients.png` | 8.6 |
| 8.8 Subgroup performance by income group | `fig_subgroup_income.png` | 8.7 |
| 8.9 Feature-set ablation | `fig_feature_set_ablation.png` | 8.8 |

## 3. URLs checked (`results/url_check.csv`, checked 2026-11-05)

* OWID data and codebook, World Bank GDP and metadata endpoints, Crossref API, Week 1 and Week 2 repositories: HTTP 200.
* 17 of 21 links returned HTTP 200 in total; the four others are the 403 publisher DOIs listed above. Crossref verification of all 14 cited references: VERIFIED (`results/reference_check.csv`); 1 candidate dropped (Pedregosa et al., 2011).

## 4. Claims in the report that are NOT printed by the code

These rest on general knowledge, on a cited paper, or on Week 2, not on a file in `results/`:

* The Kaya identity (CO2 per person = GDP per person x energy per GDP x CO2 per energy); the statement that income group is defined from a threshold on national income per person; that the World Bank classification used is the current one (stated in the Week 2 plan).
* Strengths, limitations and assumptions in Table 5.1 (including why SVM, k-NN and a neural network were not trained) are standard textbook points, not tested here.
* The reasons SMOTE is unsuitable on panels (synthetic neighbours across countries and years), and that HGB's built-in early stopping draws a random validation split (scikit-learn behaviour).
* "Some model families distort calibration" (Niculescu-Mizil and Caruana, 2005), "PR curves are more informative under imbalance" (Davis and Goadrich; Saito and Rehmsmeier), "MCC is preferable to F1 and accuracy" (Chicco and Jurman), "random splits can be too optimistic for grouped data" (Roberts et al.), "leakage is widespread" (Kapoor and Narayanan), "decoupling is not the same as a falling climate footprint" (Haberl et al.). I paraphrased the papers' well-known conclusions; I did not re-read the full texts in this session.
* "Small states and conflict-affected countries are probably MNAR": not testable, stated as an inference.
* COVID-19 affected windows ending in 2020 to 2022: background knowledge.
* Zero or negative early CO2 values for Kosovo (1994-1999) and Timor-Leste (1994-1997): checked in `data/processed/panel.csv` in this session; also stated in Week 2.
* Run times in Appendix A (about 8 and 6 minutes on 8 cores) were observed once.
* The Week 2 figures in Section 1.1 are copied into `results/week2_context.csv` from the Week 2 result files (`hypothesis_results.csv`, `h6_contingency_and_residuals.csv`).

## 5. Deviations and decisions (all logged in `docs/ml_plan.md`)

* Look-back years 1988-1989 loaded so that 5-year features exist at t = 1993 (the brief says window 1990-2022 and t from 1993; the 5-year momentum and volatility features need five years of history). This is in the committed plan itself, not a deviation.
* Three of the OWID energy columns you listed (`fossil_share_energy`, `low_carbon_share_energy`, `renewables_share_energy`) do not exist in the CO2 dataset and were dropped (`results/column_availability.csv`).
* Deviations 1 to 5 (before the test): CV re-run after a duplicate-row bug, saga and IterativeImputer convergence warnings, rehearsal mode, SE rule caveat. Deviations 6 and 7 (after the test): the post hoc analysis and duplicated rows in two test CSVs.
* The h = 5 robustness variant was run in CV only (only t = 2017 would have a five-year label in the test period).
* The F1 threshold rule and the simplicity rule were kept even though they led to unflattering outcomes.

## 6. Twelve likely viva questions

1. **Why PR-AUC and not accuracy?** About three rows in four are negatives, so always predicting "no decoupling" gets high accuracy (the prior baseline's test accuracy is 0.751, higher than the logistic regression's 0.273). PR-AUC looks only at how well positives are ranked and has a clear baseline (the prevalence).
2. **What is the purge gap and why three years?** The label looks three years ahead, so a training row at year t uses outcomes up to t+3. Leaving t+1 to t+3 unused before a validation or test block means no training label window overlaps the evaluation period.
3. **Why a time-based split and not a random split?** Neighbouring windows from one country share two years of outcomes, so a random split lets the model see nearly the same answer in training; time-based splits also mimic real forecasting.
4. **Why was logistic regression chosen when the random forest scored higher?** The selection rule, fixed before seeing results, says: take the highest mean CV PR-AUC unless a simpler model is within one standard error. The random forest scored 0.384 with SE 0.051; the logistic regression's 0.346 was above the cut-off of 0.334. On the test set the forest scored higher (post hoc), but changing the choice after seeing the test would make the test set a selection tool.
5. **Did the post hoc analysis contaminate the test set?** No model was refitted or reselected and the frozen configuration and threshold were not changed. The extra comparisons only read the saved test predictions, and they are labelled exploratory.
6. **Why were year, country and income group excluded from the features?** Year invites memorising the base rate of specific years; country identity defeats generalisation to unseen countries; income group is defined from income (circular with GDP per person) and the version used is the current one, which leaks the future. They are used only for subgroup evaluation.
7. **Why not SMOTE?** It interpolates between minority rows; on a panel the neighbours come from different countries and years, blurring time order and risking leakage. Class weights and imbalance-aware metrics were used instead.
8. **Why is the threshold so low (0.124) and is that a bug?** Not a bug. The F1-maximising threshold on out-of-fold predictions falls where flagging almost everything gives the best F1, because the model separates classes only weakly. That is why the thresholded results are poor and the report relies on PR-AUC and ROC-AUC.
9. **Why is the logistic regression no better than the income-group prior?** Most of what the linear model captures is that richer countries decouple more often, the Week 2 finding. Within income groups its skill is close to chance (for example ROC-AUC 0.46 for lower-middle income). The random forest beat the prior by 0.092 PR-AUC (interval +0.018 to +0.172), so there may be non-linear structure worth studying.
10. **How did you avoid leakage in imputation, scaling and feature selection?** All of them are steps in a scikit-learn Pipeline refitted on each training fold (median, winsorising limits, scaler, correlation filter). The forward-fill option uses only past values. A pytest check shows that medians and scaler means come from the training rows only.
11. **Why a country-clustered bootstrap?** Rows from the same country are strongly dependent; resampling rows would understate uncertainty. Resampling whole countries respects that. All intervals use 2,000 resamples and percentile limits.
12. **What are the main limitations and what would you do next?** Production-based CO2 only, a strict binary label, a small and dependent sample, a COVID-shaped test period, weak signal. Next: repeat the comparison on a later hold-out period, add calibration inside folds, try a continuous target and consumption-based emissions, and use the energy-mix shares from OWID's energy dataset.

## 7. Push commands (the repository is committed locally only)

```
git remote add origin https://github.com/KanishkVerma-046/co2-decoupling-ml-pipeline.git
git branch -M main
git push -u origin main
```

Create the empty repository on GitHub first (no README, no licence), then run the commands above from the project folder.
