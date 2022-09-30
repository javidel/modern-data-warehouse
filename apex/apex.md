# Create an insight application with APEX


## Introduction

Oracle APEX is a low-code development platform that enables you to build scalable, secure enterprise apps, with world-class features, that can be deployed anywhere. Using APEX, developers can quickly develop and deploy compelling apps that solve real problems and provide immediate value. You won't need to be an expert in a vast array of technologies to deliver sophisticated solutions. Focus on solving the problem and let APEX take care of the rest.

In this Lab we are going to use APEX to visualize the result from all the labs.

Estimated Lab Time: 15 minutes.

### Objectives

In this lab, you will:

* Create an APEX application
* Create simple charts 
* Modify a chart



### Prerequisites

This lab assumes you have done all the other labs.

## Task 1: Create Workspace

1. Let's create a new workspace in APEX before we create an application. In your Autonomous Database, go to **Tools** and then **Oracle APEX**.

![Select SQL](./images/go-to-apex.png)

2. We need to log in with the admin user. We just need to introduce the password. 

    - **Password:** Password123##

![Select SQL](./images/sign-admin.png)

3. Click on **Create Workspace**

![Select SQL](./images/create-workspace.png)

4. Click on **Existing Schema**

![Select SQL](./images/existing-schema.png)

5. Define the user and worskpace. Then click on Create Workspace

    - **Database User:** CNVG

    - **Workspace Name:** CNVG

    - **Workspace Username:** CNVG

    - **Password:** Password123##

![Select SQL](./images/define-workspace.png)

6. Click over **CNVG** to go to the new workspace.

![Select SQL](./images/new-workspace.png)

7. Sign in with the CNVG user:

    - **Workspace:** CNVG

    - **Workspace Username:** CNVG

    - **Password:** Password123##

![Select SQL](./images/sign-cnvg.png)

## Task 2: Create an application with charts

1. Let's create our application. Go to **App Builder** and click on **Create**.

![Select SQL](./images/create-app.png)

2. Select **New Application** 

![Select SQL](./images/new-app.png)

3. Let's define the name for our application. Let's call it Data Insights

![Select SQL](./images/data-insights.png)

4. Let's add a page to our application. Click on **Add Page**

![Select SQL](./images/add-page.png)

5. Click on **Dashboard**

![Select SQL](./images/add-dashboard.png)

6. This dashboard will populate 4 charts. We just need to define what we want to plot. Let's define our first chart.

![Select SQL](./images/chart1.png)

![Select SQL](./images/goto2.png)

7. Let's populate our second chart.

![Select SQL](./images/chart2.png)

![Select SQL](./images/goto3.png)

8. Let's populate our third chart

![Select SQL](./images/chart3.png)

![Select SQL](./images/finish-page.png)

9. Select check all and click on create app.

![Select SQL](./images/terminate-app.png)

10. Let's have first look into our application. Click on the **Run** button.

![Select SQL](./images/run-app.png)

11. Log in with the CNVG user. Then click on Sign in

    - **Username:** CNVG

    - **Password:** Password123##

![Select SQL](./images/log-cnvg.png)

12. Click on Dashboard

![Select SQL](./images/select-dashboard.png)

13. We can see our insight application with the reports.

![Select SQL](./images/first-report.PNG)

As you can see, we haven't defined the fourth chart. Let's modify that chart in the next task.

## Task 3: Modify a chart

1. Let's modify the last chart. We need to go back to the Application Builder tab, and click on the dashboard page.

![Select SQL](./images/dashboard-page.png)

## Acknowledgements
* **Author** - Priscila Iruela, Technology Product Strategy Director
* **Contributors** - Victor Martin Alvarez, Technology Product Strategy Director
* **Last Updated By/Date** - Priscila Iruela, September 2022

## Need Help?
Please submit feedback or ask for help using our [LiveLabs Support Forum](https://community.oracle.com/tech/developers/categories/livelabsdiscussions). Please click the **Log In** button and login using your Oracle Account. Click the **Ask A Question** button to the left to start a *New Discussion* or *Ask a Question*.  Please include your workshop name and lab name.  You can also include screenshots and attach files.  Engage directly with the author of the workshop.

If you do not have an Oracle Account, click [here](https://profile.oracle.com/myprofile/account/create-account.jspx) to create one.