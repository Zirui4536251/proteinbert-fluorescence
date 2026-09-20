# Pretrained model

The original notebook loads `epoch_92400_sample_23500000.pkl` from a local directory with automatic downloading disabled. Place that file in this directory before running the notebook.

The [official ProteinBERT repository](https://github.com/nadavbra/protein_bert) points to the [pretrained checkpoint](https://github.com/nadavbra/proteinbert_data_files/blob/master/epoch_92400_sample_23500000.pkl) and [Zenodo distribution](https://zenodo.org/records/10371965). Obtain it from a trusted upstream source; pickle files should not be loaded from untrusted providers.

Neither the pretrained checkpoint nor fine-tuned weights are bundled here. The coursework code evaluates the in-memory fine-tuned model and does not export a reusable fine-tuned checkpoint.
