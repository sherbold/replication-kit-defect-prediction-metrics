# Replication Kit

This repository serves as replication kit for the paper "Exploring the relationship between performance metrics and cost saving potential of defect prediction models". 

## Content
- `configCrossPare/` contains the configuration files for the CrossPare experiments.
- `data/` contains our (intermediate) result data.
- `plots/` contains the plot files created by the analysis (by `replication_notebook.ipynb`).
- `convert_dataframe.ipynb` has the purpose to manipulate the CrossPare result dataframe to fit the variables of our study.
- `db_credentials.env` is the credential file for a MySQL database for the storage of CrossPare results.
- `replication_notebook.ipynb` is the notebook containing the analysis of the data.

## Requirements
For the CrossPare experiments:
- Java 8
- MySQL database

For the notebooks of our study:
- Python 3.8 or higher
- jupyter-notebook or jupyter-lab
- graphviz

For Ubuntu 20.04 the setup to start a Jupyter Lab for the execution of the notebooks can be:

```    
sudo apt-get update    
sudo apt-get install python3-venv build-essential python3-dev graphviz
git clone https://github.com/sherbold/replication-kit-defect-prediction-metrics
cd replication-kit-defect-prediction-metrics/
python3 -m venv venv
source venv/bin/activate
pip install jupyterlab
jupyter lab
```

## Instructions

For the replication of our experiments there are multiple entry points.
These are possible through the intermediate data stored in `data/`.
For the full replication continue reading. 
For using our results of the CrossPare experiments go to [Create Dataset from CrossPare Results](#create-dataset-from-crosspare-results).
For the analysis of our _performance metrics vs. cost saving potential_ dataset continue at [Run Analysis](#run-analysis).

#### Run Defect Prediction Experiments in CrossPare

1. Setup the [CrossPare](https://github.com/sherbold/CrossPare) tool.
2. Load the code metric [dataset](https://zenodo.org/record/5675024/files/release-level-data.tar.gz?download=1) and unpack it (archive: 587MB, unpacked: 6.57GB).
3. Copy the `Crosspare/testdata/mynbou/` directory with the full dataset.
4. Setup a MySQL database and define the settings in a `mysql.cred` file in the CrossPare directory.
5. Run CrossPare with the folder `configCrossPare/bootstrap` as argument for the bootstrap experiment. These configurations need to be run 100 times each.
6. Run CrossPare with the folder `configCrosPare/generalization` as argument for the generalization experiment.

#### Create Dataset from CrossPare Results

The results from our run of the CrossPare experiments are saved in the files `data/database_metrics_vs_costsaving_bt_exp.csv` and `data/database_metrics_vs_costsaving_real.csv`.
To convert them into the dataframe for the evaluation, execute the `convert_dataframe.ipynb` notebook. 
For the conversion of CrossPare results directly from a MySQL database you have to make a few adjustments.
Overwrite the credentials in `db_credentials.env` with the credentials of your database.
Further, set the `USE_DATABASE` flag in the notebook to `True` and adjust the database table names before executing `convert_dataframe.ipynb`.
The resulting datasets with the variables for the study then overwrite our data in the files `data/metrics_vs_costsaving_bootstrap_experiment.csv` and `data/metrics_vs_costsaving_realistic_settings.csv`.

#### Run Analysis

Run the `replication_notebook.ipynb` notebook. Optionally, you can disable the `RERUN_LOGIT_OPTIMIZER` flag in order to directly use the optimization results stored in `data/logit_optim_grid_search.csv`. The analysis requires that the code metric data set is available in the folder `release-level-data`. 
