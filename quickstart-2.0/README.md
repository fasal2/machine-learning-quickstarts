# Getting Started
## Log-in to the Azure Portal

In your local machine, open a new browser window and sign in to the **Azure Portal** (<http://portal.azure.com>).</br>
If you already have an Azure account and you are logged-in in the browser, then please use a **private browser window** to log in to Azure portal.  

### Azure Credentials
Here are your credentials to login to **[Microsoft Azure](<http://portal.azure.com>)** and access the On Demand Lab

**Username** : <inject key="AzureAdUserEmail"></inject>

**Password** : <inject key="AzureAdUserPassword"></inject>

# Automated Machine Learning in Azure Notebook VM

This quickstart will show you how to use Automated Machine Learning with [Azure ML Python SDK](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-configure-auto-train) to train, deploy, and test the best model from within Azure Notebook VMs.

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

In this quickstart, you will use compute resources provided by Azure Machine Learning service to remotely train a set of models using Automated Machine Learning using [Azure ML Python SDK](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-configure-auto-train). Next, you will evaluate performance of each model and pick the best performing model to deploy as a scoring web service hosted by Azure Container Instance. Finally, you will test the scoring web service: (1) by make direct calls on service object, (2) by calling the service end point (Scoring URI) over http.

As part of this quickstart, we will be building a regression model to predict Taxi Fares in New York City. We will use a preprocessed labeled training data with features such as number of passengers, trip distance, datetime, holiday information and weather information.


# Exercise 1: Azure Compute Instance Setup

At a high level, here are the setup tasks you will need to perform to prepare your Azure Compute Instance Environment (the detailed instructions follow):

1. Create a Compute Instance in your Azure subscription

2. Import the Quickstart Notebooks

## Task 1: Create a Compute Instance

1. Log into [Azure Portal](https://portal.azure.com/) and open the machine learning workspace: quick-starts-ws-XXXXX or quick-starts-ws

2. Select **Compute, Compute instances** in the left navigation and then select **+ New**

   ![Select Create New Notebook VM in Azure Portal](images/01.png)

3. Provide the following information and then select **Create**

    a. Compute name: `quick-starts-nvm`

    b. Virtual machine type: `CPU (Central Processing Unit)`

    c. VM machine size: `Standard_DS3_V2`

   ![Create New Compute Instance](images/02.png)

   > Note: If the Compute Instance names should be unique within an Azure Region notification appears, choose a different name that is unique to your environment.
  
4. Wait for the Compute Instance to be ready, it will take around 5 minutes.

## Task 2: Import the Quickstart Notebooks

1. Select the Compute Instance: **quick-starts-nvm** and then select **Jupyter** open icon, to open Jupyter Notebooks interface.

   ![Open Jupyter Notebooks Interface](images/03.png)

2. Select **New, Terminal** as shown to open the terminal page.

   ![Open Terminal Page](images/04.png)
  
3. Run the following commands in order in the terminal window:

   a. `mkdir quick-starts`
   
   b. `cd quick-starts`
   
   c. `git clone https://github.com/solliancenet/machine-learning-quickstarts.git`
   
      ![Clone Github Repository](images/05.png)
   
   d. Wait for the import to complete.

# Exercise 2: Complete the Quickstart

1. From the Jupyter Notebooks interface, navigate to the `quick-starts->machine-learning-quickstarts->quickstart-2.0->notebooks` folder where you will find all your quickstart files.

2. Open notebook: **automl-with-AML.ipynb**

3. Follow the instructions within the notebook to complete the quickstart.
