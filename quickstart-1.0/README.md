# Getting Started
## Log-in to the Azure Portal

In your local machine, open a new browser window and sign in to the **Azure Portal** (<http://portal.azure.com>).</br>
If you already have an Azure account and you are logged-in in the browser, then please use a **private browser window** to log in to Azure portal.  

### Azure Credentials
Here are your credentials to login to **[Microsoft Azure](<http://portal.azure.com>)** and access the On Demand Lab

**Username** : <inject key="AzureAdUserEmail"></inject>

**Password** : <inject key="AzureAdUserPassword"></inject>

# Automated Machine Learning in Azure Portal

In this quickstart you will learn how to create, run, and explore automated machine learning experiments in the [Azure portal](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-create-portal-experiments) without a single line of code. You will also learn to register and deployed the best trained model, all within the Azure Portal.
## Automated Machine Learning with Azure Machine Learning
Automated machine learning picks an algorithm and hyperparameters for you and generates a model ready for deployment. There are several options that you can use to configure automated machine learning experiments.

Configuration options available in automated machine learning:

- Select your experiment type: Classification, Regression or Time Series Forecasting
- Data source, formats, and fetch data
- Choose your compute target: local or remote
- Automated machine learning experiment settings
- Run an automated machine learning experiment
- Explore model metrics
- Register and deploy model

You can create and run automated machine learning experiments in code using the [Azure ML Python SDK](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-configure-auto-train) or if you prefer a no code experience, you can also Create your automated machine learning experiments in the [Azure portal](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-create-portal-experiments).

# Quickstart Overview

In this quickstart, you learn how to create, run, and explore automated machine learning experiments in the [Azure portal](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-create-portal-experiments) without a single line of code. Next, from within the Azure portal, you will deploy a scoring web service on Azure Container Instance to make predictions using the registered model.

As part of this quickstart, we will be building a regression model to predict Taxi Fares in New York City. We will use a preprocessed labeled training data with features such as number of passengers, trip distance, datetime, holiday information and weather information.

# Exercise 1: Setup New Automated Machine Learning Experiment

## Task 1: Create New Automated Machine Learning Experiment

1. In Azure Portal, open the machine learning workspace: `quick-starts-ws-XXXXX` or `quick-starts-ws`

2. Select **Try the new Azure Machine Learning studio**

    ![Select Launch the new Azure Machine Learning studio.](images/01.png 'Launch the new Azure Machine Learning studio')

    > Note that for the first time you will be asked to select the subscription and machine learning workspace from a drop down list before opening the new studio interface.

3. From the studio, select **Create new, Automated ML run**

    ![Create new Automated ML run from Azure Machine Learning studio.](images/02.png 'New Automated ML run')

4. This will open a `Create a new automated machine learning experiment` page

## Task 2: Upload and Review Training Data

1. From the `Select dataset` step, select **Create dataset, From web files**

    ![Open select dataset from local files.](images/03.png 'Select dataset')

2. Provide Name: `nyc-taxi-sample-data`. In the Web URL field provide the following URL for the training data file and then select **Next**

    `https://introtomlsampledata.blob.core.windows.net/data/nyc-taxi/nyc-taxi-sample-data.csv`

    ![Upload nyc-taxi-sample-data.csv from a web URL.](images/04.png 'Upload dataset')

3. On the `Settings and preview panel`, set the column headers drop down to `All files have same headers`. Scroll to right to observe the target column: `totalAmount`. After you are done reviewing the data, select **Next**

    ![Scroll right to review dataset.](images/05.png 'Review dataset')

4. Select columns from the dataset to include as part of your training data. Leave the default selections and select **Next**

    ![Select columns from the dataset to include as part of your training data.](images/06.png 'Select columns')

5. Confirm the dataset details and select **Create**

    ![Confirm the details of the dataset you uploaded and then select Create.](images/07.png 'Confirm and create the dataset')

6. Select the dataset you created and then select **Next**

    ![Select the dataset you created and then select.](images/08.png 'Select dataset')

## Task 3: Configure Run and Create Training Compute

1. Provide an experiment name: `auto-ml-exp`

2. Select target column: `totalAmount`

3. Select **Create new compute**. This will open the `Create new compute` dialog pane on the right hand side.

    ![Configure the run by providing the experiment name, selecting the target column and open the create new compute dialog.](images/09.png 'Configure Run')

4. In the `New compute cluster` dialog provide the following information and then select **Create**

    - Compute name: `auto-ml-compute`
    - Virtual machine type: `CPU (Central Processing Unit)`
    - Virtual Machine size: `Standard_DS3_v2`
    - Minimum number of nodes: 1
    - Maximum number of nodes: 1

    ![The figure shows the values provided in the create new compute dialog.](images/10.png 'Create new compute')

5. The compute will take couple of minutes to create. Wait for the compute to be ready.

6. In the `Configure run` step, select the compute target: **auto-ml-compute** and then select **Next**

    ![Complete the run configuration by selecting the compute target, auto-ml-compute, and then selecting next.](images/11.png 'Select compute target')

## Task 4: Setup Task type and Settings

1. Select task type: **Regression**

    ![Select task type, regression.](images/12.png 'Select task type')

2. Open `Additional configurations` dialog by selecting **View additional configuration settings**

    ![Open the additional configurations dialog by selecting the view additional configuration settings link.](images/13.png 'Open Additional configurations dialog')

3. In the `Additional configurations` dialog, configure the following and then select **Save**

    - Primary metric: `Spearman correlation`
    - Training job time (hours): `1`
    - Metric score threshold: `0.933`

    ![Select primary metric as Spearman correlation and setup the exit criteria.](images/14.png 'Additional configurations dialog')

    > Note that the `Spearman correlation` measures the monotonic relationships between the predicted value and actual value. In this case, the model with spearman_correlation score closest to 1 is the best model.

    > Note that we are setting a metric score threshold to limit the training time. In practice, for initial experiments, you will typically only set the training job time to allow AutoML to discover the best algorithm to use for your specific data.

# Exercise 2: Start and Monitor Experiment

## Task 1: Start Experiment

1. Select **Finish** to start running the experiment

    ![Select Finish to start running the experiment.](images/16.png 'Start Experiment')

## Task 2: Monitor Experiment

1. The experiment will run for about *5-10 min*

2. In the **Details** tab, observe the **run status** of the job.

    ![Run Details tab showing run status.](images/17.png 'Run Details')
  
3. Select the **Models** tab, and observe the various algorithms the AutoML is evaluating. You can also observe the corresponding Spearman correlation scores for each algorithm.

    ![Models tab showing model performance metric.](images/18.png 'Models')

    > Note that we have set a metric score threshold to limit the training time. As a result you might see only one algorithm in your models list.

4. Select **Details** and wait till the run status becomes **Completed**.

    ![Run Details tab showing run status.](images/19.png 'Run Details')

# Exercise 3: Review Best Model's Performance

## Task 1: Review Best Model Performance Metrics

1. The `Details` tab shows the `Best model summary`. Next, select **Algorithm name** to review the model details.

    ![Run Details tab showing best model.](images/20.png 'Best Model')
  
2. From the `Model details` tab, to view the various metrics to evaluate the best model performance, select **View all other metrics**.

    ![Model Details tab showing model summary.](images/21.png 'Model Details')

3. Review the model performance metrics and then select **Close**.

    ![Model performance metrics.](images/21_2.png 'Run Metrics')

## Task 2: Review Best Model Predictions

1. Next, select **Visualizations** to review Predicted Taxi Fare vs True Taxi Fare for your model.

    ![Model Details tab showing model summary.](images/21_3.png 'Model Details')

2. Review the model predictions. Next, select **Explanations**.

    ![Graph of model predicted value versus true value.](images/22.png 'Model Predictions')

## Task 3: Review Best Model Explanations

1. Review the model features that are most influential in making predictions. The graph shows that the `trip distance`, as expected, is the most influential feature followed by features like `hour_of_day` and `day_of_week`. Next, select **Model details** to return back to the `Model details` tab.

    ![Bar graph of top 8 important features.](images/23.png 'Model Explanations')

# Exercise 4: Deploy Best Model

## Task 1: Deploy Model

1. Select **Deploy model**

    ![Select Deploy model.](images/24.png 'Deploy model')

2. In the `Deploy a model` dialog, provide the following information, and then select select **Deploy**:

    - Name: `nyc-taxi-predict`
    - Description: `Predict NYC Taxi Fares!`
    - Compute type: `ACI`

    ![Provide information in the Deploy a model dialog and then select Deploy.](images/25.png 'Deploy a Model')

3. Wait for the service to be deployed. It can take couple of minutes for the deployment to finish.

## Task 2: View Endpoint Consumption Information

1. To view the deployed service, select the **Endpoints** section in your Azure Portal Workspace.

    ![Select Endpoints section in your Azure Portal Workspace.](images/26.png 'Service Details')

2. Select the deployed service: **nyc-taxi-predict**

    ![Endpoints list showing the deployed service, nyc-taxi-predict.](images/27.png 'Endpoints')

    *Note: you have to select the text of the service name to open the deployment details page*

3. Select **Consume** tab to view the basic consumption information:

    - REST endpoint URL

    ![Consume tab showing basic consumption information for the deployed endpoint.](images/28.png 'Consume')

# Exercise 5: Challenge Experiment

In the current experiment, the pipeline of MaxAbsScaler, RandomForest gave us the best performing model with the spearman correlation score of: 0.933. Can you expand the number of iterations for the Automated Machine Learning experiment to see if we can find a better performing model? Note that in each iteration, a new machine learning model is trained with your data.

# Exercise 7: Clean-up

1. Select **Endpoints** section in your Azure Portal Workspace and then select **nyc-taxi-predict, Delete**

    ![Select the deployed endpoint, nyc-taxi-predict, and then select delete.](images/29.png 'Delete Endpoint')

2. Select **Compute, Compute clusters** section in your Azure Portal Workspace and then select **auto-ml-compute, Delete**

    ![Select the training cluster, auto-ml-compute, and then select delete.](images/30.png 'Delete Training Cluster')
