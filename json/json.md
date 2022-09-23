# Load JSON tweets in the Autonomous Database


## Introduction

Autonomous Database supports JSON data natively in the database. You can use NoSQL-style APIs to develop applications that use JSON document collections without needing to know SQL or how the documents are stored in the database.

In this Lab you are going to learn how to upload JSON data and how to run some queries over them.

Estimated Lab Time: 15 minutes.

### Objectives

In this lab, you will:

* Create a Object Storage bucket
* Upload JSON data
* Create Autonomous Credentials
* Query JSON data


### Prerequisites

This lab assumes you have created the Autonomous Data Warehouse database in the previous lab.

## Task 1: Upload JSON tweets into Object Storage

1. Once you have downloaded the JSON containing some tweets, we need to upload them into Object Storage. First we need to create a bucket.

    ![Go to Buckets](./images/create-bucket.png)

2. Click on **Create Bucket**

    ![Create Bucket](./images/create-bucket2.png)

3. Set the name for the bucket. We are going to call it **json_data**. Then click the **create** button.

    ![Define Bucket](./images/create-bucket3.png)

4. Select the **json_data** bucket.

    ![Select Bucket](./images/select-bucket.png)

5. Click on the **upload** button.

    ![Select Bucket](./images/select-upload.png)

6. Select the JSON file we just downloaded and then click on **upload**

    ![Select Bucket](./images/upload-json.png)

7. We should see the JSON file there.

    ![Select Bucket](./images/file-uploaded.PNG)
    


## Task 2: Create credential for Autonomous Database

1. Now we have the tweets available in the Object Storage. Now we need to create a **credential**. This credential willl allow the Autonomous Database to authenticate against the Object Storage Service. Click on your profile icon and then on your email name.

    ![Find credentials](./images/go-to-credential.png)

2. Let's create a new token for the Autonomous Database. Click on **Auth Token**.

    ![Open credentials](./images/go-to-token.png)

3. Click on **Generate Token**

    ![Create credentials](./images/generate-token.png)

4. It will show a popup like this. This token will be shown only once. Click **Show** to show the token. **Save it** in a notepad or a secure place for later. It will never be shown again.

    ![Show token](./images/show-token.png)

5. Click **copy** and store it in a secure place. Then you can click **close**.

    ![Copy token](./images/save-token.png)

6. We have stored the credential, now we need to find and store the location of where the data is stored. We will share this info with the Autonomous Database so it can load it. Let's go back to the Object Storage.

    ![Go to Object](./images/go-to-object.png)

7. Select the JSON bucket we already created.

    ![Select JSON](./images/select-json-bucket.png)

8. Let's find the information from the tweets. From the menu of the file, select **View Object Details**

    ![View Details](./images/get-json-details.png)

9. You will find the url with the JSON file location. **Save this url** as we are going to need it for loading it. Then click **cancel** to exit.

    ![Save URL](./images/get-url-json.png)

## Task 3: Load JSON into Autonomous Database

1. As we have the credential created and we know the url where we store our JSON data, now we can proceed to load this data. Let's go to our Autonomous Data Warehouse

    ![Go to ADW](./images/go-to-adb.png)

2. Select our MODERNDW database

    ![Choose ADW](./images/choose-adw.png)

3. Go to **Database Actions**

    ![Choose DB ACtions](./images/go-to-actions.png)

4. We need to connect with the **CNVG** and not with the ADMIN user. Let's log out first

    ![log out](./images/sign-out.png)

5. Click on **Sign in**.

    ![log ing](./images/sign-in.png)

## Task 4: Run queries over JSON



## Acknowledgements
* **Author** - Javier de la Torre, Principal Data Mangagement Specialist
* **Contributors** - Priscila Iruela, Technology Product Strategy Director
* **Last Updated By/Date** - Javier de la Torre, Principal Data Mangagement Specialist

## Need Help?
Please submit feedback or ask for help using our [LiveLabs Support Forum](https://community.oracle.com/tech/developers/categories/livelabsdiscussions). Please click the **Log In** button and login using your Oracle Account. Click the **Ask A Question** button to the left to start a *New Discussion* or *Ask a Question*.  Please include your workshop name and lab name.  You can also include screenshots and attach files.  Engage directly with the author of the workshop.

If you do not have an Oracle Account, click [here](https://profile.oracle.com/myprofile/account/create-account.jspx) to create one.