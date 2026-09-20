# Method and editorial notes

## Task and attribution

This is sequence-level fluorescence regression using pretrained ProteinBERT, not a new protein-language-model architecture or pretraining run. The assignment identifies the official signal-peptide fine-tuning demo as a template. The adapted task uses `OutputType(False, 'numeric')` rather than a binary classification output.

## Original implementation

- Drop missing rows and exact duplicate rows separately in the training and test CSVs.
- Hold out 10% of the cleaned training file with split seed 42; no explicit TensorFlow or NumPy training seed is set.
- Load the local ProteinBERT checkpoint and expose hidden-layer outputs via the package helper.
- Configure dropout 0.5 and staged fine-tuning: frozen pretrained layers, full-model training, then one final epoch at sequence length 1024. The other stages use length 512 and batch size 32; the package can adjust the final-stage batch size.
- Use ReduceLROnPlateau (patience 1, factor 0.25, minimum learning rate 1e-5) and EarlyStopping (patience 2, restore best weights).
- Create a length-512 model for test prediction; report MSE, RMSE, MAE, R² and Pearson correlation. The evaluation helper's `output_spec` argument is retained although unused internally.

## Code/log discrepancy

The saved source passes `lr=1e-4`, `lr_with_frozen_pretrained_layers=1e-2`, and `final_lr=1e-5`. The archived log instead shows an initial learning rate of **2e-4 in all three stages**. The cause cannot be established from the notebook alone; it may involve environment behavior or code changes after execution. Neither the source settings nor a guessed explanation should be presented as confirmed effective hyperparameters.

The log records 20 frozen-stage epochs, 14 full-model epochs and one final long-sequence epoch. Final-stage validation loss is 0.3842, higher than the best displayed full-model-stage validation loss of 0.0769. Do not claim that the final stage improved performance. The exact relationship between the saved code and weights that produced the archived test scores remains unverified.

## Optimizer compatibility patch

The source monkey-patches Keras Adam: it renames `lr` to `learning_rate`, fills missing attributes, and supplies placeholder `get_weights`/`set_weights` methods. Those placeholders store an object; they do not establish correct restoration of optimizer variables. The patch is retained for traceability, not endorsed as full Keras 3 compatibility. It affects Adam globally within the kernel. Restart the kernel before unrelated experiments.

Original Python, TensorFlow, Keras and ProteinBERT versions and package commit were not recorded. Upstream documentation describes older tested dependencies; this project does not claim that installing arbitrary current package versions will reproduce the run. Only static syntax and file checks were performed.

## Why no confusion matrix?

The assignment requests a confusion matrix, but the submitted implementation predicts a continuous target. A classification confusion matrix requires discrete classes. No class definition, threshold or full prediction array is saved. The repository therefore reports the regression metrics and observed-versus-predicted plot that actually exist. It does not claim to satisfy the assignment's confusion-matrix request.

If a binned analysis is later required, define bins using training data or a justified domain criterion, apply the same bins to observed and predicted values, and label the result as a secondary discretized-regression analysis. That would be a new analysis, not an output of the archived submission.

## Scope and limitations

- Training and test target distributions differ (recorded means 3.1806 and 2.0825); this does not establish the cause or split design.
- R² is not classification accuracy, and Pearson correlation is not Spearman rank correlation. No uncertainty estimates, repeated seeds or external validation are reported.
- Full-row deduplication does not establish sequence independence; no homology-grouped validation or cross-split overlap check is implemented.
- Missing raw CSVs, exact package versions and fine-tuned checkpoint prevent independent reproduction of the archived results here.

## Editorial changes

The single large code cell was separated into named sections with concise English explanations. Machine-specific paths became repository-relative paths. Repetitive comments and the unsupported claim of full Keras 3 compatibility were removed. Model logic and numeric settings were retained, including the optimizer patch; no new algorithm, threshold, metric or experiment was introduced.

Code execution counts and outputs were cleared. The archived result image is shown in a labeled Markdown cell, and original console output is stored separately. Initial screenshot-only Markdown was omitted as redundant to the actual code output. No model training, inference or metric recomputation occurred during editing.
