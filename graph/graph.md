# Identify influencers using Oracle Graph




## Introduction

Oracle Graph allows to discover patterns in the data. We will use the tweets to see how the users talk with each other in order to get more information.

Estimated Lab Time: 30 minutes.

### Objectives

In this lab, you will:

* Create a Graph Server
* Populate a graph
* Use the Page Rank algorithm to find influencers


### Prerequisites

This lab assumes you have created the Autonomous Data Warehouse database and you have loaded the JSON tweets from Lab 2.

## Task 1: Load Friends JSON Data

1. We have a new dataset which has information about twitter's users followers. We are going to use this data to build a graph. First we need to upload this file into the object storage. Go to **storage** and **buckets**

    ![Go to Storage](./images/go-to-storage.png)

2. Select our **json_data** bucket.

    ![Select Bucket](./images/select-bucket.png)

3. Click on **Upload** button.

    ![Select upload](./images/select-upload.png)

4. Select the friend_of.json and click upload.

    ![Select upload](./images/upload-json.png)

5. Click close.

    ![Select upload](./images/click-close.png)

6. Let's get the URL of the JSON Object. We will need it for later.

    ![Select upload](./images/view-object-details.png)

7. **Store** the URL for later usage.

    ![Select upload](./images/get-url.png)

8. We need to go back to JSON. Let's create a new JSON Collection.

    ![Select upload](./images/back-to-json.png)

9. Click on create new collection.

    ![Select upload](./images/new-collection.png)

10. Set the new name for the collection as **friend_of** and click **create**.

    ![Select upload](./images/create-collection.png)

11. Go back to SQL, for loading the data.

    ![Select upload](./images/back-to-sql.png)

12. Load the JSON using the COPY_COLLECTION utility.

        <copy> BEGIN 
            DBMS_CLOUD.COPY_COLLECTION(    
            collection_name => 'friend_of', 
            credential_name=>'json_cred',   
            file_uri_list => 'YOUR_URL',
            format => '{"recorddelimiter" : "0x''01''", "unpackarrays" : "TRUE", "maxdocsize" : "10240000"}'
            );
            END;
            /
        </copy>

    ![Select upload](./images/load-json.png)

## Task 2: Create a VCN for the Graph Server

1. We need to create a Virtual Cloud Network (VCN) before we provision the Graph Server. Go to Networking and then Virtual Cloud Network.

    ![Find VCN](./images/find-vcn.png)

2. Click on **Start VCN Wizard**

    ![VCN Wizard](./images/vcn-wizard.png)

3. Select **Create VCN with Internet Connectivity** and click on **Start VCN Wizard**

    ![StartVCN Wizard](./images/start-wizard.png)

4. Define the VCN Name as **vcn_graph**. Then click on **Next**.

    ![StartVCN 1](./images/vcn1.png)

5. Keep the default configuration. Click on **Create**

    ![StartVCN 2](./images/create-vcn.png)

6. We need to open the port **7007** for the Graph Server. Let's go back to the VCN configuration.

    ![StartVCN 3](./images/back-to-vcn.png)

7. Select the just created VCN, **vcn_graph**

    ![Select VCN](./images/select-vcn.png)

8. Select the public vcn.

    ![Select VCN](./images/select-public.png)

9. Select the default Security List.

    ![Select VCN](./images/default-security.png)

10. Click on **Add Ingress Rule**

    ![Select VCN](./images/add-ingress.png) 

11. Set the following rule for opening the port 7007, then click on **Add Ingress Rules**: 

    - **CIDR:** 0.0.0.0/0
    
    - **IP Protocol:** TCP

    - **Destination Port Range:** 7007

    - **Description:** Graph Server

    ![Select VCN](./images/define-ingress.png) 

## Task 3: Create Oracle Graph Server

1. Before we create the Graph Server, we need to generate a ssh key. For that we are going to use the cloud shell. 

    ![Open Shell](./images/open-cloud-shell.png) 

    ![Shell](./images/shell-open.png) 

2. Once the cloud shell has started, run the following commands to generate a ssh key. Press Enter twice for no passphrase

        <copy> 
            mkdir .ssh
        </copy>

        <copy> 
            cd .ssh
        </copy>

        <copy> 
            ssh-keygen -b 2048 -t rsa -f graphkey
        </copy>

    ![Shell](./images/key-generated.png) 

3. We will need the public key when creating the Graph Server. In order to get the public key, you can run the following command:

        <copy> 
            cat graphkey.pub
        </copy>

    ![Shell](./images/public-key-content.png) 

4. Now we are ready to provision the Graph Server. We can find it in the marketplace.

    ![Shell](./images/go-to-marketplace.png) 

5. In the marketplace, look for **Graph**. There you can select **Oracle Graph Server and Client**.

    ![Shell](./images/search-graph.png)

6. Select that you had reviewed the terms and restrictions and click on **Launch Stack**.

    ![Shell](./images/launch-stack.png)

7. Leave the default configuration and click **Next**.

    ![Shell](./images/next-launch1.png)

8. Now configure the Graph Server:


    
    - **Availability Domain:** Choose any available

    - **Shape:** VM.Standard.E2.1

    - **SSH Public Key:** Copy from the cloud shell

    ![Shell](./images/graph-config1.png)

9. Select the VCN and the **public subnet** we already created.

    ![Shell](./images/graph-config2.png)

10. Finally we have to define the JDBC connection for our Autonomous Database

        <copy> 
            jdbc:oracle:thin:@moderndw_low?TNS_ADMIN=/etc/oracle/graph/wallets
        </copy>
    

    ![Shell](./images/url-jdbc.png)

11. Click on **Next**.

    ![Shell](./images/next-graph.png)

12. Click on **Create**.

    ![Shell](./images/create-graph-server.png)

13. This will create a terraform job for creating this instance.

    ![Shell](./images/job-running.png)

14. You should see it as succeed.

    ![Shell](./images/job-finish.png)

15. The Graph connect needs the Autonomous Wallet to connect. We need to download it and upload it into the Graph Server. Go to the Autonomous Database and click on **DB Connection**. 

    ![Shell](./images/db-connection.png)

16. Click on Download Wallet.

    ![Shell](./images/download-wallet.png)

17. Set the same password: **Password123##** and click download.

    ![Shell](./images/set-password.png)

18. Now we are going to upload the wallet to the **Cloud Shell**.

    ![Shell](./images/upload-wallet.png)

19. Select the wallet and click on **Upload**

    ![Shell](./images/shell-upload-wallet.png)

20. We are going to use SCP to move the wallet from the Cloud Shell into the Graph Server. Before we need the IP. Let's find it under compute.

    ![Shell](./images/find-server.png)

21. Copy the Public IP. We are going to need it for the next exercise

    ![Shell](./images/copy-ip.png)

22. Go back to the Cloud shell. Run the following command to copy the wallet into the Graph Server. Remember to use **YOUR IP**. 

        <copy> 
            scp -i .ssh/graphkey Wallet_MODERNDW.zip opc@YOUR_IP:/etc/oracle/graph/wallets
        </copy>

    ![Shell](./images/wallet-uploaded.png)
    

23. Now we just need to unzip it on the Graph Server. We need to connect via ssh and unzip it.
        
        <copy> 
            ssh -i .ssh/graphkey opc@YOUR_IP
        cd /etc/oracle/graph/wallets/
        unzip Wallet_MODERNDW.zip
        chgrp oraclegraph *
        </copy>

## Task 4: Create a Graph

1. The Graph Server is created, now we need to populate the Graph. For simplicity, we are going to create a simpler table of twitter users and we are going to create a view on top of the friend_of JSON Collection. Let's create the view first. Go to your Autonomous Database and click on **Database Actions**

    ![Shell](./images/go-to-dbactions.png)

2. Go to JSON

    ![Shell](./images/select-json.png)

3. Select the **friend_of** collection and click on create view.

    ![Shell](./images/select-view.png)

4. Add **all columns** and click on **Create**

    ![Shell](./images/create-view.png)

5. Go back to sql

    ![Shell](./images/back-to-sql2.png)

6. We need a primary for our view. Also we are going to create a simplified table for our twitter users and we are going to create a primary key too.

        <copy> 
            ALTER VIEW FRIEND_OF_VIEW ADD CONSTRAINT FRIEND_OF_VIEW_PK PRIMARY KEY ( ID ) DISABLE;
            create table twitter_user as select name, identifier, description, location from MV_TWEETS;
            ALTER TABLE twitter_user ADD CONSTRAINT twitter_user_pk PRIMARY KEY (identifier);
        </copy>

    ![Shell](./images/prepare-data.PNG)

7. Now we can finally create the Graph. Let's go back to our cloud shell to connect to the Graph Server. If you have disconnected you can conenct again via ssh.

        <copy> 
            ssh -i .ssh/graphkey opc@YOUR_IP
        </copy>

8. Let's connect with the **opg4py** utility. We are going to use the **cnvg** user and the password **Password123##**

        <copy> 
            opg4py -b https://localhost:7007 -u cnvg
        </copy>

    ![Shell](./images/connect-graph.PNG)

    ![Shell](./images/graph-prompt.PNG)

9. Let's define the Graph.

        <copy> 
            statement = '''
            CREATE PROPERTY GRAPH "influencer"
            VERTEX TABLES (
            TWITTER_USER
            )
            EDGE TABLES (
            friend_of_view
            SOURCE KEY(user_id) REFERENCES TWITTER_USER
            DESTINATION KEY(is_frend_of) REFERENCES TWITTER_USER
            )
            '''

        </copy>

    ![Shell](./images/define-graph.PNG)

10. Let's execute the definition.

        <copy> 
            session.prepare_pgql(statement).execute()
        </copy>

    ![Shell](./images/execute-definition.PNG)

11. Let's store the graph in a variable.

        <copy> 
            graph=session.get_graph("influencer")
        </copy>

    ![Shell](./images/store-variable.PNG)

12. Now let's run a simple query.

        <copy> 
            graph.query_pgql("""
            SELECT a.name,a.description, a.location,b.name as "friend of"
            FROM MATCH (a)-[e]->(b)
            """).print()
        </copy>

    ![First Query](./images/query1.PNG)


## Task 5: Find influencers

1. As we have the Graph loaded, let's use the Page Rank ALgorithm to determine influencers in our community.

        <copy> 
            analyst.pagerank(graph)
        </copy>

2. Now let's see the results. Let's run a simple query.

        <copy> 
            graph.query_pgql("""
            SELECT a.name,a.description, a.pagerank
            FROM MATCH (a)
            ORDER BY a.pagerank DESC
            """).print()

        </copy>

    ![LIst Influencers](./images/query-influencers.PNG)

## Acknowledgements
* **Author** - Priscila Iruela, Technology Product Strategy Director
* **Contributors** - Victor Martin Alvarez, Technology Product Strategy Director
* **Last Updated By/Date** - Priscila Iruela, September 2022

## Need Help?
Please submit feedback or ask for help using our [LiveLabs Support Forum](https://community.oracle.com/tech/developers/categories/livelabsdiscussions). Please click the **Log In** button and login using your Oracle Account. Click the **Ask A Question** button to the left to start a *New Discussion* or *Ask a Question*.  Please include your workshop name and lab name.  You can also include screenshots and attach files.  Engage directly with the author of the workshop.

If you do not have an Oracle Account, click [here](https://profile.oracle.com/myprofile/account/create-account.jspx) to create one.