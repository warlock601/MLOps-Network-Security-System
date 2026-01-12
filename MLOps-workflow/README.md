### Model Experiment Tracker with MLFlow

- First in requirements.txt, we'll add mlflow & dagshub so that these will be installed and then imported.

- In model_trainer.py, we will add the logic for Tracking the experiment using MLFlow. First we call a function which pass best_model, classification_train_metric as arguments self.track_mlflow(best_model,classification_train_metric).
