# Operationalizing Machine Learning Final Project

In this project, I build an end-to-end ML model pipeline with the Bank Marketing dataset. I discuss how to train a model through the Azure ML Studio, publishing the pipeline, and consuming it so that we can make predictions through an API request.



## Architectural Diagram
*TODO*: Provide an architectual diagram of the project and give an introduction of each step. An architectural diagram is an image that helps visualize the flow of operations from start to finish. In this case, it has to be related to the completed project, with its various stages that are critical to the overall flow. For example, one stage for managing models could be "using Automated ML to determine the best model". 



## Key Steps


### 1. Authentication
I must create a Service Principal using az. However, since I used the VM, I was unable to cdo this because I did not have the prpoert permissions.

### 2. Automated ML Experiment
The next step is to create an experiment using Automated ML, configure a compute cluster, and use that cluster to run the experiment. Using the Bank Marketing dataset,  I trained the data using a binary classification model (the target column being `y`) to identify customers who were influenced by the bank's marketing campaign. Below are screenshots of the uploaded data and the finished experiment.
![registered dataset](sample_screenshots/registered-data.PNG)
![completed experiment](sample_screenshots/completed-experiment.PNG)

We are then able to see all the models that the experiment ran and select the best one. In my experiment, the model used StandardScalerWrapper and XGBoostClassifier algorithms to get an AUC weighted score of 0.94505.
![best model](sample_screenshots/best-model.PNG)

### 3. Deploy the Best Model
Using the model mentions above, I deployed it using an Azurre Container Instance and enabled authentication which will allow me to interact with the HTTP API service through POST requests.

### 4. Enable Logging
After deploying the model, I must enable Application Insights so that I can retrieve logs to monitor the model. I adjust the given `logs.py` script to match the workspace configuration of my Azure account and enable insights.
![app insights](sample_screenshots/app-insights.PNG)
![logs](sample_screenshots/logs.PNG)

### 5. Swagger Documentation
Next I consume the model through Swagger. It provides helpful information about the API requests and responses when provided the endpoint. Using the Swagger JSON file provided by Azure, I serve it through a given python script (with some changes to the port) and we are able to see the Swagger UI on a local container.
![swagger](sample_screenshots/swagger.PNG)

### 6. Consume Model Endpoints
Now that the endpoint is available, I can now interact with the trained model. Running the `endpoint.py` script to get a prediction for two example customers, we can see that the results were `yes` and `no`.
![endpoints](sample_screenshots/endpoint.PNG)

### 7. Create, Publish, and Consume a Pipeline
Using the Azure SDK for Python, I create a pipeline using AutoMLStep. I then submit the pipeline with the experiment above, as seen below:
![pipeline created](sample_screenshots/pipeline-created.PNG)

After the experiment is complete, I submit the pipeline to create the REST endpoint so that I can interact with the model. We can also see on the right that the endpoint is active.
![pipeline endpoint](sample_screenshots/pipeline-endpoint.PNG)
![rest active](sample_screenshots/rest-active.PNG)

We can see that the pipeline consists of two parts: the dataset and the AutoML module. I then send a POST request to the active endpoint. Although the RunDetails widget was not working for me, you can see that it is completed within ML Studio (I accidentally missed this screenshot but you can see the finished run in the video).
![bank module](sample_screenshots/bank-module.PNG)
![run detail](sample_screenshots/run-detail.PNG)



## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:



