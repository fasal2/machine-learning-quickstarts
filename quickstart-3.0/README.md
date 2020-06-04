# Getting Started
## Log-in to the Azure Portal

In your local machine, open a new browser window and sign in to the **Azure Portal** (<http://portal.azure.com>).</br>
If you already have an Azure account and you are logged-in in the browser, then please use a **private browser window** to log in to Azure portal.  

### Azure Credentials
Here are your credentials to login to **[Microsoft Azure](<http://portal.azure.com>)** and access the On Demand Lab

**Username** : <inject key="AzureAdUserEmail"></inject>

**Password** : <inject key="AzureAdUserPassword"></inject>


# Azure Machine Learning Designer

[Azure Machine Learning designer](https://docs.microsoft.com/en-us/azure/machine-learning/service/concept-designer) (preview) gives you a cloud-based interactive, visual workspace that you can use to easily and quickly prep data, train and deploy machine learning models. It supports Azure Machine Learning compute, GPU or CPU. Machine Learning designer also supports publishing models as web services on Azure Kubernetes Service that can easily be consumed by other applications.

In this quickstart, we will be using a subset of NYC Taxi & Limousine Commission - green taxi trip records available from [Azure Open Datasets](https://azure.microsoft.com/en-us/services/open-datasets/). The data is enriched with holiday and weather data. Based on the enriched dataset, we will learn to use the Azure Machine Learning Graphical Interface to process data, build, train, score, and evaluate a regression model to predict NYC taxi fares. To train the model, we will create Azure Machine Learning Compute resource. We will also learn to deploy the model as a scoring webservice to Azure Kubernetes Compute. Finally, we will test the deployed webservice, and review how to consume the deployed web service. We will do all of this from the Azure Machine Learning designer without writing a single line of code.

# Exercise 1: Setting up your environment 

If a lab environment has not be provided for you, this lab provides the instructions to get started in your own Azure Subscription.

The labs have the following requirements:
- Azure subscription. You will need a valid and active Azure account to complete this Azure lab. If you do not have one, you can sign up for a [free trial](https://azure.microsoft.com/en-us/free/).

## Azure Quotas Required
The quickstart depend on the capability to utilize a certain quantity of Azure resources, for which your Azure subscription will need to have sufficient quota available.

The following are the specific quotas required, if your subscription does not meet the quota requirements in the region in which you will perform the quickstart, you will need to request a quota increase thru Azure support:

Compute-VM
- Quota: Standard Dv2 Family vCPUs
- Provider: Microsoft.Compute
- SKU family: Dv2 Series
- Required Limit: 14

Compute-VM
- Quota: Total Regional vCPUs
- Provider: Microsoft.Compute
- SKU family: Dv2 Series
- Required Limit: 14

## Prerequisites

- Create an Azure resource group named: `QuickStarts`. See [Create Resource Groups](https://docs.microsoft.com/en-us/azure/azure-resource-manager/manage-resource-groups-portal) for details on how to create the resource group.

- Create an Azure Machine Learning service workspace, **enterprise edition**, named: `quick-starts-ws`. See [Create an Azure Machine Learning Service Workspace](https://docs.microsoft.com/en-us/azure/machine-learning/service/setup-create-workspace) for details on how to create the workspace.

Next, we will upfront create two Azure Machine Learning Computes, one for running machine learning experiments and the other for deploying trained models. Creating computes can take up to 5 minutes, thus we can start the creation and move on to the quickstart to conserve time.

## Task 1: Create Azure Machine Learning Compute

Create a compute target in the workspace `quick-starts-ws` to run your Azure Machine Learning experiments.

1. In Azure Portal, open the machine learning workspace: `quick-starts-ws`

2. Select **Try the new Azure Machine Learning studio**

    ![Select Launch the new Azure Machine Learning studio.](images/01.png 'Launch the new Azure Machine Learning studio')

    > Note that for the first time you will be asked to select the subscription and machine learning workspace from a drop down list before opening the new studio interface.

3. From the studio, select **Compute, Compute clusters, + New**. This will open the `New compute cluster` dialog on the right. In the dialog, enter the following information and then select **Create**:

   a. Compute name: `qs-compute`

   b. Virtual machine type: `CPU (Central Processing Unit)`

   c. Virtual machine size: `Standard_DS3_v2`

   d. Minimum number of nodes: `1`

   e. Maximum number of nodes: `1`

    ![Image highlights the steps to open the create new training cluster dialog, and shows the information to provide for the different fields in the dialog.](images/02.png 'New Training Cluster')

    > Note: Continue working on the lab as you do not need the compute service until Exercise 3.

## Task 2: Create Kubernetes Service Compute

Next, we will create a Kubernetes Service Compute to publish the trained model as web service.

1. From the studio, select **Compute, Inference clusters, + New**. This will open the `New inference cluster` dialog on the right. In the dialog, enter the following information and then select **Create**:

   a. Compute name: `nyc-taxi-service`

   b. Region: `East US`

   c. Virtual machine size: `Standard_D3_v2`

   d. Cluster purpose: `Production`

   e. Number of nodes: `3`

    ![Image highlights the steps to open the create new inference cluster dialog, and shows the information to provide for the different fields in the dialog.](images/03.png 'New Inference Cluster')

    > Note: Continue working on the lab as you do not need the Kubernetes service until Exercise 7.

## Task 3: Download the Training Data File

1. Download the training data file [nyc-taxi-sample-data.csv](https://quickstartsws9073123377.blob.core.windows.net/azureml-blobstore-0d1c4218-a5f9-418b-bf55-902b65277b85/quickstarts/nyc-taxi-data/nyc-taxi-sample-data.csv) on your local computer.

# Exercise 2: Register Dataset with Azure Machine Learning studio

## Task 1: Upload Dataset

1. From the studio, select **Datasets, + Create dataset, From local files**. This will open the `Create dataset from local files` dialog on the right. Provide an unique name to the dataset and then select **Next**

   ![Image highlights the steps to open the create dataset from local files dialog.](images/04.png 'Create dataset from local files')

2. In the `Datastore and file section` dialog, perform the following steps:

    1. Select **Previously created datastore**
    2. Select **workspaceblobstore (Default)**
    3. Select **Browse** to locate and select the file `nyc-taxi-sample-data.csv` on your local machine
    4. Select **Select datastore**
    5. Select **Next**

    ![Upload nyc-taxi-sample-data.csv from your local machine.](images/05.png 'Upload dataset')

## Task 2: Preview Dataset

1. On the `Settings and preview` panel, set the column headers drop down to `All files have same headers`. Scroll to right to observe the target column: `totalAmount`. After you are done reviewing the data, select **Next**

    ![Scroll right to review dataset.](images/06.png 'Review dataset')

## Task 3: Select Columns

1. Select columns from the dataset to include as part of your training data. Leave the default selections and select **Next**

    ![Select columns from the dataset to include as part of your training data.](images/07.png 'Select columns')

## Task 4: Create Dataset

1. Confirm the dataset details and select **Create**

    ![Confirm the details of the dataset you uploaded and then select Create.](images/08.png 'Confirm and create the dataset')

# Exercise 3: Create New Training Pipeline

## Task 1: Open Pipeline Authoring Editor

1. From the studio, select **Designer, +**. This will open a `visual pipeline authoring editor`.

   ![Image highlights the steps to open the pipeline authoring editor.](images/09.png 'Pipeline Authoring Editor')

## Task 2: Setup Compute Target

1. In the settings panel on the right, select **Select compute target**.

    ![Image highlights the link to select to open the setup compute target editor.](images/10.png 'Setup Compute Target')

2. In the `Set up compute target` editor, select the existing compute target: **qs-compute**, and then select **Save**.

    ![Image shows how to select the existing compute target named qs-compute.](images/11.png 'Setup Compute Target')

## Task 3: Add Dataset

1. Select **Datasets** section in the left navigation. Next, select **My Datasets, nyc-taxi-sample-data** and drag and drop the selected dataset on to the canvas.

    ![Image shows the dataset, nyc-taxi-sample-data, added to the canvas.](images/12.png 'Add Dataset')

## Task 4: Split Dataset

1. Select **Data Transformation** section in the left navigation. Follow the steps outlined below:

    1. Select the **Split Data** prebuilt module

    2. Drag and drop the selected module on to the canvas

    3. Fraction of rows in the first output dataset: **0.7**

    4. Connect the `Dataset` to the `Split Data` module

    ![Image shows the steps to add and configure the Split Data module.](images/13.png 'Split Data Module')

    > Note that you can run the pipeline at any point to peek at the outputs and activities. Running pipeline also generates metadata that is available for downstream activities such selecting column names from a list in selection dialogs.

## Task 5: Initialize Regression Model

1. Select **Machine Learning Algorithms** section in the left navigation. Follow the steps outlined below:

    1. Select the **Boosted Decision Tree Regression** prebuilt module

    2. Drag and drop the selected module on to the canvas

    3. Minimum number of samples per leaf node: **1**

    4. Learning rate: **0.1**

    ![Image shows the steps to add and configure the Boosted Decision Tree Regression module.](images/14.png 'Boosted Decision Tree Regression Module')

## Task 6: Setup Train Model Module

1. Select **Model Training** section in the left navigation. Follow the steps outlined below:

    1. Select the **Train Model** prebuilt module

    2. Drag and drop the selected module on to the canvas

    3. Connect the `Boosted Decision Tree Regression` module to the first input of the `Train Model` module

    4. Connect the first output of the `Split Data` module to the second input of the `Train Model` module

    5. Select the **Edit column** link to open the `Label column` editor

    ![Image shows the steps to add and configure the Train Model module.](images/15.png 'Train Model Module')

2. The `Label column` editor allows you to specify your `Label or Target column`. Type in the label column name **totalAmount** and then select **Save**.

    ![Image shows the label column editor and how to provide the label column name.](images/16.png 'Label Column')

## Task 7: Setup Score Model Module

1. Select **Model Scoring & Evaluation** section in the left navigation. Follow the steps outlined below:

    1. Select the **Score Model** prebuilt module

    2. Drag and drop the selected module on to the canvas

    3. Connect the `Train Model` module to the first input of the `Score Model` module

    4. Connect the second output of the `Split Data` module to the second input of the `Score Model` module

    ![Image shows the steps to add and configure the Score Model module.](images/17.png 'Score Model')

    > Note that `Split Data` module will feed data for both model training and model scoring. The first output (0.7 fraction) will connect with the `Train Model` module and the second output (0.3 fraction) will connect with the `Score Model` module.

## Task 8: Setup Evaluate Model Module

1. Select **Model Scoring & Evaluation** section in the left navigation. Follow the steps outlined below:

    1. Select the **Evaluate Model** prebuilt module

    2. Drag and drop the selected module on to the canvas

    3. Connect the `Score Model` module to the first input of the `Evaluate Model` module

    ![Image shows the steps to add and configure the Evaluate Model module.](images/18.png 'Evaluate Model')

# Exercise 4: Run Training Pipeline

## Task 1: Create Experiment and Run Pipeline

1. Select **Submit** to open the `Setup pipeline run` editor.

    ![Image shows where to select the run button to open the setup pipeline run editor.](images/19.png 'Run Pipeline')

2. In the `Setup pipeline run` editor, select **Create new**, provide `New experiment name:` **designer-run**, and then select **Submit**.

    ![Image shows how to provide the experiment name in the setup pipeline run editor and start the pipeline run.](images/20.png 'Run Pipeline')

3. Wait for pipeline run to complete. It will take around **5-10 minutes** to complete the run.

# Exercise 5: Visualize Training Results

## Task 1: Visualize the Model Predictions

1. Select **Score Model, Outputs + logs, Visualize** to open the `Score Model result visualization` dialog.

    ![Image shows how to open the score model result visualization dialog.](images/21.png 'Score Model Results')

2. Observe the predicted values under the column **Scored Labels**. You can compare the predicted values (`Scored Labels`) with actual values (`totalAmount`).

    ![Image shows the score model result visualization dialog.](images/22.png 'Model Predictions')

## Task 2: Visualize the Evaluation Results

1. Select **Evaluate Model, Outputs + logs, Visualize** to open the `Evaluate Model result visualization` dialog.

    ![Image shows how to open the evaluate model result visualization dialog.](images/23.png 'Evaluate Model Results')

2. Evaluate the model performance by reviewing the various evaluation metrics, such as **Mean Absolute Error**, **Root Mean Squared Error**, etc.

    ![Image shows the evaluate model result visualization dialog.](images/24.png 'Evaluation Metrics')

# Exercise 6: Realtime Inference Pipeline

## Task 1: Create Pipeline

1. Select **Create inference pipeline, Real-time inference pipeline** to create a new inference pipeline.

    ![Image shows how to create a new real-time inference pipeline.](images/25.png 'Create Real-time Inference Pipeline')

## Task 2: Run Pipeline

1. Select **Submit** to open the `Set up pipeline run` editor.

    ![Select run to open the set up pipeline run editor.](images/26.png 'Real-time Inference Pipeline')

2. Select **Existing experiment** from the list: **designer-run**, and then select **Submit**.

    ![Image shows how to select the experiment name in the set up pipeline run editor and start the pipeline run.](images/27.png 'Setup Pipeline Run')

3. Wait for pipeline run to complete. It will take around **3-5 minutes** to complete the run.

# Exercise 7: Deploy Web Service on Kubernetes Service Compute

## Task 1: Deploy Web Service

1. After the inference pipeline run is finished, select **Deploy** to open the `Set up real-time endpoint` editor.

    ![Select deploy to open the set up real-time endpoint editor.](images/28.png 'Deploy')

2. In the `Set up real-time endpoint` editor, provide the following information and then select **Deploy**.

    1. Select **Deploy new real-time endpoint**

    2. Real-time endpoint name: **nyc-taxi-fare-predictor**

    3. Select an existing compute target name: **nyc-taxi-service**

    ![Image shows the information provided in the set up real-time endpoint editor.](images/29.png 'Deploy Web Service')

    > Note that **nyc-taxi-service** is the Kubernetes Service Compute created in the prerequisites section.

## Task 2: Review Deployment State of the Web Service

1. To view the deployed web service, select the **Endpoints** section in your Azure Portal Workspace.

2. Select the deployed web service: **nyc-taxi-fare-predictor** to open the deployment details page.

    ![Image highlights the deployed web service, nyc-taxi-fare-predictor.](images/31.png 'Endpoints')

    > Note: You have to select the text of the service name to open the deployment details page.

3. The deployment is complete when the `Deployment state` is **Healthy**. Wait for the deployment to complete before proceeding.

    ![Image shows the details of the deployed web service, nyc-taxi-fare-predictor.](images/31_2.png 'Service Details')

    > Note: You may have to periodically refresh the page to update the deployment status.

## Task 3: Review how to Consume the Deployed Web Service

1. Select **Consume** tab, to observe the following information:

    1. `Basic consumption info` displays the **REST endpoint**, **Primary key**, and **Secondary key**.

    2. `Consumption option` shows code samples in **C#**, **Python**, and **R** on how to call the endpoint to consume the webservice.

    ![Image highlights the two key regions in the consume tab of the deployment details, the first region shows basic consumption information, and the second region shows code samples of how to consume the deployed service.](images/32.png 'Consume')

# Exercise 8: Challenge Experiment

Is there another regression model that can give us an improved evaluation score on the `Root Mean Square Error (RMSE)` metric?  The `Boosted Decision Tree Regression` gave us an RMSE score of **3.92**. Experiment with other models like the `Neural Net Regression` model and evaluate if you can train a better performing model on the RMSE metric. Please note that the objective is to minimize the RMSE score.

# Exercise 9: Cleanup Resources

## Task 1: Delete Web Service

1. Select the **Endpoints** section in your Azure Portal Workspace.

2. Select **nyc-taxi-fare-predictor, Delete** to delete the web service.

    ![Image shows how to delete the web service, nyc-taxi-fare-predictor.](images/33.png 'Endpoints')

## Task 2: Delete Compute Cluster

1. Select the **Compute, Compute clusters** section in your Azure Portal Workspace.

2. Select **qs-compute, Delete** to delete the training cluster.

    ![Image shows how to delete the training cluster, qs-compute.](images/34.png 'Training Clusters')

## Task 3: Delete Inference Cluster

1. Select the **Compute, Inference clusters** section in your Azure Portal Workspace.

2. Select **nyc-taxi-service, Delete** to delete the inference cluster.

    ![Image shows how to delete the inference cluster, nyc-taxi-service.](images/35.png 'Inference Clusters')
