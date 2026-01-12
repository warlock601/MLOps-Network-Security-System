# Model Experiment Tracker with MLFlow

- First in requirements.txt, we'll add mlflow & dagshub so that these will be installed and then imported. Once we get the best model, for that best model whatever classification metrics we get, we need to make sure that we track the entire thing in MLFlow. We will create track_mlflow function, which    will be called in model_trainer. We will pass the best_model, classificationmetric as arguments which will have precision_score, f2_score, recall_score.
- To log the metric, __mlflow.log_metric will be used to log the metrics in experiments__ where we specify the metrics like f1_score, precision_score, recall_score etc.
- __mlflow.sklearn.log_model(best_model,"model")__ is used to log the best model.
```bash
def track_mlflow(self,best_model,classificationmetric):
        mlflow.set_registry_uri("https://dagshub.com/krishnaik06/networksecurity.mlflow")
        tracking_url_type_store = urlparse(mlflow.get_tracking_uri()).scheme
        with mlflow.start_run():
            f1_score=classificationmetric.f1_score
            precision_score=classificationmetric.precision_score
            recall_score=classificationmetric.recall_score

            

            mlflow.log_metric("f1_score",f1_score)
            mlflow.log_metric("precision",precision_score)
            mlflow.log_metric("recall_score",recall_score)
            mlflow.sklearn.log_model(best_model,"model")
            # Model registry does not work with file store
            if tracking_url_type_store != "file":

                # Register the model
                # There are other ways to use the Model Registry, which depends on the use case,
                # please refer to the doc for more information:
                # https://mlflow.org/docs/latest/model-registry.html#api-workflow
                mlflow.sklearn.log_model(best_model, "model", registered_model_name=best_model)
            else:
                mlflow.sklearn.log_model(best_model, "model")
```

- In model_trainer.py, we will call the track_mlflow function where we write the code for tracking with MLFlow in the ModelTrainer class. This is the logic that we'll be using for tracking ML experiments using MLFlow.
- __self.track_mlflow(best_model,classification_train_metric)__ is used to track the experiments with mlflow. Traking is for train metrics here. If we want to track for Test metrics as well then: __self.track_mlflow(best_model,classification_test_metric)__
```bash
## Track the experiements with mlflow
        self.track_mlflow(best_model,classification_train_metric)


        y_test_pred=best_model.predict(x_test)
        classification_test_metric=get_classification_score(y_true=y_test,y_pred=y_test_pred)

        self.track_mlflow(best_model,classification_test_metric)

        preprocessor = load_object(file_path=self.data_transformation_artifact.transformed_object_file_path)
            
        model_dir_path = os.path.dirname(self.model_trainer_config.trained_model_file_path)
        os.makedirs(model_dir_path,exist_ok=True)

        Network_Model=NetworkModel(preprocessor=preprocessor,model=best_model)
        save_object(self.model_trainer_config.trained_model_file_path,obj=NetworkModel)
```
