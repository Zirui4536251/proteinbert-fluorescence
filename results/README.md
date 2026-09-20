# Recorded outputs

All results in this folder come from the submitted HW9 notebook. No model was run while preparing this repository.

- `recorded_metrics.csv`: five test metrics, rounded as printed in the original run.
- `recorded_prediction_preview.txt`: the original ten-row table display; sequences are truncated by pandas. This is not a complete prediction dataset.
- `original_console_output.txt`: the saved training and evaluation log, including the learning-rate values actually displayed.
- `../figures/predicted_vs_observed_original.png`: the original embedded PNG, extracted without changing its bytes.

Full test predictions, raw data, fine-tuned weights and an environment lockfile were not attached. Consequently, no new confusion matrix, residual analysis, Spearman correlation or confidence intervals have been calculated. The saved `Correlation` value is Pearson correlation, computed with `numpy.corrcoef`.
