# Predict Customer Churn with AutoML


## Introduction

Oracle Machine Learning AutoML UI (OML AutoML UI) is a no-code user interface supporting automated machine learning for both data scientist productivity and non-expert user access to powerful in-database algorithms. Oracle Machine Learning AutoML UI automates model building with minimal user input – you just have to specify the data and the target in what’s called an experiment and the tool does the rest. However, you can adjust some settings, such as the number of top models to select, the model selection metric, and even specific algorithms. Similarly, you can deploy models from OML AutoML UI as REST endpoints to Oracle Machine Learning (OML) Services in just a few clicks.

In this Lab we are going to use all the data we have for creating a Machine Learning model to predict customer churn or which customer could be a candidate to use our affinity card.

Estimated Lab Time: 15 minutes.

### Objectives

In this lab, you will:


* Create a Machine Learning Model
* Store the Machine Learning model in Autonomous for running SQL with it

### Prerequisites

This lab assumes you have created the Autonomous Data Warehouse database in the previous lab.

## Task 1: Prepare the data

1. **Oracle AutoML requires an unique table to understand how the data is related and to define the column to predict**. For that we are going to create an unique table called **dw\_table**. Go to **SQL** and **Run** the following statements. Remember, you need to be the **CNVG** user and not the admin user.

    ```
        <copy> 
            create table dw_table as
            select a.*,b.sentiment,b.emotion
            from revenue a, SENTIMENT_RESULTS b
            where a.cust_key=b.name;
        </copy>
    ```
    
    Check that the **statements has being completed successfully**.

    ![Create Warehouse](./images/create-dw-table.png)

## Task 2: Create the Model

1. As we have our data ready, let's create our **Machine Learning Model**. Go to your **Autonomous Database** dashboard and click on **Tools** and then **Oracle ML User Administration**. Click finally on **Open Oracle ML User Administration** to open the ML development environment.

    ![Create Warehouse](./images/open-ml.png)

2. Log in with the **ADMIN** user.

    - **Username:** ADMIN
        ```
            <copy>ADMIN</copy>
        ```
    
    - **Password:** Password123##
        ```
            <copy>Password123##</copy>
        ```

    ![Create Warehouse](./images/log-in.png)

3. You will see that the **CNVG** user is already there. Click the **Home** button to sign in into the AutoML page.

    ![Create Warehouse](./images/go-home.png)

4. Now we can sign in with the **CNVG** user.

    - **Username:** CNVG
        ```
            <copy>CNVG</copy>
        ```
    
    - **Password:** Password123##
        ```
            <copy>Password123##</copy>
        ```

    ![Create Warehouse](./images/login-automl.png)

5. Click on the **Home** menu of the ML page. Select **AutoML Experiments**.

    ![Create Warehouse](./images/select-automl.png)

6. Click on **Create** button.

  ![Create Warehouse](./images/create-experiment.png)

7. Let's define our experiment. Fill the following information:

    - **Name:** Predict Churn
        ```
            <copy>Predict Churn</copy>
        ```
    
    - **Data Source:** DW_TABLE

    - **Predict:** AFFINITY_CARD

    - **Case ID:** CUST_KEY

  ![Create Warehouse](./images/prepare-model.png)

8. We don't need to fill more information, click on **Start** and select **Faster Results**.

    ![Create Warehouse](./images/faster-results.png)

9. Once it is finish, we can see which are the columns or features that have bigger impact in our model. **We can see that emotion is very important**.

    ![Create Warehouse](./images/importance.png)

## Task 3: Store and Query the model inside the Autonomous Database

1. Once the job has finished, we can see the **result and compare the different algorithms**. Note: we see a 100% accuracy. Consider that it is a small dataset built for demo purposes. Let's select the **Decission Tree Model** and let's create a **Notebook**.

    ![Create Warehouse](./images/create-notebook.png)

2. Click **OK**.

    ![Create Warehouse](./images/click-ok.png)

3. Click on **Open Notebook**.

    ![Create Warehouse](./images/open-notebook.png)

4. We need to define the name of the model to be stored inside of the database. For that we need to add a new parameter called **model\_\_\_name** in the box called **Build MODEL\_NAME\_TITLE model**. You can **copy** from this page. Once you **have add** it, you can click on the **Play** button to **Run all the paragraphs**.

    ```
        <copy> 
            ,model_name='CHURN_MODEL'
        </copy>
    ```

    ![Create Warehouse](./images/store-model.png)

5. We can **validate** if our model has been stored inside of the database. We are going to **run** a simple query, finding from our customer if they are likely to have our **affinity card and their probability**.

    ```
        <copy> 
            select AFFINITY_CARD,cust_key,PREDICTION(cnvg.churn_model USING *),PREDICTION_PROBABILITY(cnvg.churn_model USING *) 
            from dw_table
        </copy>
    ```

    ![Create Warehouse](./images/query-model.png)

## Acknowledgements
* **Author** - Javier de la Torre, Principal Data Mangagement Specialist
* **Contributors** - Priscila Iruela, Technology Product Strategy Director
* **Last Updated By/Date** - Javier de la Torre, Principal Data Mangagement Specialist

## Need Help?
Please submit feedback or ask for help using our [LiveLabs Support Forum](https://community.oracle.com/tech/developers/categories/livelabsdiscussions). Please click the **Log In** button and login using your Oracle Account. Click the **Ask A Question** button to the left to start a *New Discussion* or *Ask a Question*.  Please include your workshop name and lab name.  You can also include screenshots and attach files.  Engage directly with the author of the workshop.

If you do not have an Oracle Account, click [here](https://profile.oracle.com/myprofile/account/create-account.jspx) to create one.