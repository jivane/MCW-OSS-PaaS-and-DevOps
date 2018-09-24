![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
OSS PaaS and DevOps
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
April 2018
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

- [OSS PaaS and DevOps hands-on lab step-by-step](#oss-paas-and-devops-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Exercise 1: Run starter application](#exercise-1-run-starter-application)
    - [Task 1: Connect to your Lab VM](#task-1-connect-to-your-lab-vm)
    - [Task 2: Grant permissions to Docker](#task-2-grant-permissions-to-docker)
    - [Task 3: Integrate GitHub into VS Code](#task-3-integrate-github-into-vs-code)
    - [Task 4: Clone the starter application](#task-4-clone-the-starter-application)
    - [Task 5: Launch the starter application](#task-5-launch-the-starter-application)
  - [Exercise 2: Migrate the database to Cosmos DB](#exercise-2-migrate-the-database-to-cosmos-db)
    - [Task 1: Provision Cosmos DB using the MongoDB API](#task-1-provision-cosmos-db-using-the-mongodb-api)
    - [Task 2: Update database connection string](#task-2-update-database-connection-string)
    - [Task 3: Pre-create and scale collections](#task-3-pre-create-and-scale-collections)
    - [Task 4: Import data to the API for MongoDB using mongoimport](#task-4-import-data-to-the-api-for-mongodb-using-mongoimport)
    - [Task 5: Install Azure Cosmos DB extension for VS Code](#task-5-install-azure-cosmos-db-extension-for-vs-code)
    - [Task 6: Decrease collection throughput](#task-6-decrease-collection-throughput)
  - [Exercise 3: Containerize the app](#exercise-3-containerize-the-app)
    - [Task 1: Create an Azure Container Registry](#task-1-create-an-azure-container-registry)
    - [Task 2: Install Docker extension in VS Code](#task-2-install-docker-extension-in-vs-code)
    - [Task 3: Create Docker image and run the app](#task-3-create-docker-image-and-run-the-app)
    - [Task 4: Run the containerized App](#task-4-run-the-containerized-app)
    - [Task 5: Push image to Azure Container Registry](#task-5-push-image-to-azure-container-registry)
  - [Exercise 4: Set up Web App for Containers](#exercise-4-set-up-web-app-for-containers)
    - [Task 1: Provision Web App for Containers](#task-1-provision-web-app-for-containers)
    - [Task 2: Navigate to the deployed app](#task-2-navigate-to-the-deployed-app)
  - [Exercise 5: Configure CI/CD pipeline](#exercise-5-configure-cicd-pipeline)
    - [Task 1: Prepare GitHub account for service integrations](#task-1-prepare-github-account-for-service-integrations)
    - [Task 2: Open connection to Jenkins](#task-2-open-connection-to-jenkins)
    - [Task 3: Configure Continuous Integration with Jenkins](#task-3-configure-continuous-integration-with-jenkins)
    - [Task 4: Trigger CI build](#task-4-trigger-ci-build)
    - [Task 5: Install Docker on the Jenkins VM](#task-5-install-docker-on-the-jenkins-vm)
    - [Task 6: Add an Azure service principal for Jenkins](#task-6-add-an-azure-service-principal-for-jenkins)
    - [Task 7: Add continuous delivery to Jenkins build job](#task-7-add-continuous-delivery-to-jenkins-build-job)
    - [Task 8: Trigger CI-CD pipeline](#task-8-trigger-ci-cd-pipeline)
  - [Exercise 6: Create Azure Function for order processing](#exercise-6-create-azure-function-for-order-processing)
    - [Task 1: Provision a Function App](#task-1-provision-a-function-app)
    - [Task 2: Configure storage queues](#task-2-configure-storage-queues)
    - [Task 3: Create Cosmos DB trigger function](#task-3-create-cosmos-db-trigger-function)
    - [Task 4: Create Queue function](#task-4-create-queue-function)
  - [Exercise 7: Create Logic App for sending SMS notifications](#exercise-7-create-logic-app-for-sending-sms-notifications)
    - [Task 1: Create Free Twilio account](#task-1-create-free-twilio-account)
    - [Task 2: Create Logic App](#task-2-create-logic-app)
  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete Azure resource groups](#task-1-delete-azure-resource-groups)
    - [Task 2: Delete WebHooks and Service Integrations](#task-2-delete-webhooks-and-service-integrations)

# OSS PaaS and DevOps hands-on lab step-by-step

## Abstract and learning objectives

In this hands-on lab, you will implement a solution for integrating and deploying complex Open Source Software (OSS) workloads into Azure PaaS. You will migrate an existing MERN (MongoDB, Express.js, React.js, Node.js) stack application from a hosted environment into Azure Web App for Containers, migrate a MongoDB instance into Cosmos DB, enhance application functionality using serverless technologies, and fully embrace modern DevOps tools.

At the end of this hands-on lab, you will be better able to migrate and deploy OSS applications into Azure PaaS using modern DevOps methodologies and Docker containers.

## Overview

Best For You Organics Company is one of the leading health food suppliers in North America, serving customers in Canada, Mexico, and the United States. They have a MERN stack web application which they host on-premises and are looking to migrate their OSS application into Azure. They will be creating a custom Docker image for their application and using a Jenkins continuous integration/continuous delivery (CI/CD) pipeline to deploy the application into a Web App for Containers instance. In addition, they will be setting up Azure Cosmos DB, using the MongoDB APIs, so they do not have to make application code changes to leverage the power of Cosmos DB.

In this hands-on lab, you will assist with completing the OSS application and database migrations into Azure. You will create a custom Docker image, provision an Azure Container Registry, push the image to the registry, and configure the CI/CD pipeline to deploy the application to a Web App for Containers instance. You will also help them implement functionality enhancements using serverless architecture services in Azure.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![This diagram consists of icons that are connected by arrows. On the left, the Developer icon (VS Code) points in a linear fashion to the GitHub Repo and Jenkins icons. The previous two icons are enclosed in a box labeled CI/CD Pipeline. Jenkins points to Web App for Containers on the right. Various arrows point from Web App for Container to: Azure Container Registry (a double-sided arrow); Logic Apps (a linear arrow that also points from Logic Apps to Customers); Customers (a linear arrow); and Azure Cosmos DB (a double-sided arrow that also points from Azure Cosmos DB to Azure Functions with another double-sided arrow).](media/solution-architecture-diagram.png "Solution architecture diagram")

The solution begins with developers using Visual Studio Code (VS Code) as their code editor, so they can leverage its rich integration with GitHub, Docker, and Azure. From a high level, developers will package the entire OSS application inside a custom Docker container using the Docker extension in VS Code. The image will be pushed to an Azure Container Registry as part of a continuous integration/continuous delivery (CI/CD) pipeline using GitHub and Jenkins. This Docker image will then be deployed to a Web App for Containers instance, as part of their continuous delivery process using The Azure App Service Jenkins plugin.

The MongoDB database will be imported into Azure Cosmos DB, using mongoimport.exe, and access the database from the application will continue to use the MongoDB APIs. The database connection string in the application will be updated to point to the new Cosmos DB.

Serverless architecture will be applied to order processing and customer notifications. Azure Functions will be used to automate the processing of orders. Logic Apps will be applied to send SMS notifications, via a Twilio connector, to customers informing them that their order has been processed and shipped.

## Requirements

1. Microsoft Azure subscription must be pay-as-you-go or MSDN
    - Trial subscriptions will *not* work
2. Linux virtual machine configured with:
    - Visual Studio Code
    - Azure CLI
    - Docker
    - Node.js and npm
    - MongoDB Community Edition

## Exercise 1: Run starter application

Duration: 30 minutes

In this exercise, you will create a local copy of the starter application on your Lab VM, add some sample data to the local MongoDB database, and run the application.

### Task 1: Connect to your Lab VM

In this task, you will create an RDP connection to your Lab VM. If you are already connected, skip to [Task 2](#task-2-grant-permissions-to-docker).

1. Navigate to the Azure portal and select **Resource groups** from the left-hand menu, then select the **hands-on-lab-SUFFIX resource group** from the list. If there are too many, enter "hands-on-lab" into the filter box to reduce the resource groups displayed the list

    ![Resource groups is highlighted in the navigation pane of the Azure portal. On the Resource groups blade, hands-on-lab is highlighted in the filter box, and hands-on-lab is highlighted in the search results.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image17.png "Azure Portal")

2. Next, select **LabVM** from the list of available resources

    ![LabVM is highlighted in the list of available resources.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image18.png "Select LabVM")

3. On the **LabVM** blade, copy the Public IP address from the Essentials area on the Overview screen

    ![Public IP address is highlighted in the top menu on the LabVM blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image19.png "LabVM blade")

4. Open a Remote Desktop Client (RDP) application and enter or paste the Public IP address of your Lab VM into the computer name field

5. Select **Connect** on the Remote Desktop Connection dialog

6. Select **Yes** to connect, if prompted that the identity of the remote computer cannot be verified

    ![This is a screenshot of the Remote Desktop Connection prompt about connecting to the remote despite the identity of the remote computer being unverified. Yes is selected.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image20.png "Remote Desktop Connection dialog box")

7. Enter the following credentials (or the non-default credentials if you changed them):

    - **User name:** demouser
    - **Password:** Password.1!!

        ![The credentials above are entered in the Login to xrdp dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image21.png "Login to xrdp dialog box")

8. Select **OK** to log into the Lab VM

### Task 2: Grant permissions to Docker

In this task, you will grant permissions to the demouser account to access the Unix socket needed to communicate with the Docker engine.

1. On your Lab VM, open a bash shell.

2. At the command prompt, enter the following command:

    ```sh
    sudo usermod -a -G docker $USER
    ```

3. After running the command, you will need **completely log out of the Lab VM** and log back in (if in doubt, reboot)

4. After logging back in, run the following command to test that the demouser account has proper permissions:

    ```sh
    docker run hello-world
    ```

    ![Screenshot of the bash shell command sudo usermod -a pG docker \$USER output. Includes output from running the test command, docker run hello-world.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image22.png "Bash Shell")

### Task 3: Integrate GitHub into VS Code

In this task, you will install the GitHub extension in VS Code, and configure a service integration with your GitHub account. This integration will allow you to push your code changes to GitHub directly from VS Code.

1. On your Lab VM, open **VS Code**

2. In VS Code, select the **Extensions** icon from the left-hand menu, enter "github" into the **Extensions** search box, and select the **GitHub** extension

    ![Extensions is highlighted in the Visual Studio Code navigation pane, github is entered in the Extensions search box, and GitHub is highlighted below.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image23.png "Visual Studio Code navigation pane")

3. Select **Install** in the Extension: GitHub window

    ![Install is highlighted in the Extension: GitHub window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image24.png "Install the GitHub extension")

4. Close VS Code

    > Note: VS Code must be restarted to enable the extension

5. To connect VS Code with your GitHub account, you need to generate a Personal access token

6. Open a browser window and navigate to your GitHub account (<https://github.com>)

7. Within your GitHub account, select **your user profile icon** in the top right, then select **Settings** from the menu

    ![The user profile icon is highlighted at the top left of the GitHub account page, and Settings is highlighted in the submenu.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image25.png "Select your account settings")

8. On the Settings screen, select **Developer settings** at the bottom of the Personal settings menu on the left-hand side of the screen

    ![Developer settings is highlighted at the bottom of the Personal settings menu on the Settings screen.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image26.png "Select Developer settings")

9. On the Developer settings page, select **Personal access tokens** from the left-hand menu

    ![Personal access tokens is highlighted on the Developer settings page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image27.png "Select Personal access tokens")

10. Select **Generate new token**

    ![The Generate new token button is highlighted on the Personal access tokens page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image28.png "Select Generate new token")

11. Enter a token description, such as "VS Code Integration", and then check the box next to **repo** under **Select scopes**, which will select all the boxes under it

    ![On the New personal access token page, VS Code Integration is entered under Token description, and the check boxes next to and under repo are selected under Select scopes.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image29.png "Select the scopes")

12. Select **Generate token** near the bottom of the screen

    ![Generate token button](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image30.png "Generate token button")

13. Select the **copy** button next to the token that is generated

    ![The copy button that is next to the new personal access button is highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image31.png "Copy the new personal access button")

    > Important: Make sure you copy the new personal access token before you navigate away from the screen, or you will need to regenerate the token. Save the copied token by pasting it into a text editor for future reference. This will also be used as your password when pushing changes to GitHub.

14. Open **VS Code** on your Lab VM

15. Select the **View** menu, then select **Command Palette** from the menu

    ![View is highlighted in the menu, and Command Palette is highlighted below.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image32.png "Visual Studio Code window, View menu")

16. In the box that appears at the top center of the VS Code window, enter "Set Personal Access Token," then select **GitHub: Set Personal Access Token**, when it appears

    ![GitHub: Set Personal Access Token is highlighted below Set Personal.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image33.png "Select GitHub: Set Personal Access Token")

17. Paste the Personal access token you copied from GitHub into the box, and press **Enter**

    ![The Personal access token is pasted in the box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image34.png "Paste the Personal access token")

18. VS Code is now connected to your GitHub account

19. Close VS Code

### Task 4: Clone the starter application

In this task, you will clone the starter application, creating a local copy on your Lab VM.

1. On your Lab VM, open a browser, and navigate to your GitHub account (<https://github.com>)

2. Within your GitHub account, navigate to the forked copy of the `mcw-oss-paas-devops` application page, select **Clone or download**, then select the **copy** link next to the web URL

    ![The Clone or download button is selected on the forked copy of the mcw-oss-paas-devops application page, and the web URL is displayed below it.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image35.png "Select Clone or download")

3. Open a new bash shell, and enter the following command:

    ```sh
    cd ~
    ```

4. Next, enter the following command, replacing [EMAIL] with the email address you used when creating your GitHub account. This will associate your git email address with the commits made from the Lab VM.

    ```sh
    git config --global user.email "[EMAIL]"
    ```

    ![The above command to associate your git email address with the commits made from the Lab VM is displayed in the bash terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image36.png "bash terminal window")

5. At the prompt, enter the following command, replacing [CLONE-URL] with URL you copied from GitHub in step 2 above:

    ```sh
    git clone [CLONE-URL]
    ```

    ![The git clone command is displayed in the bash terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image37.png "bash terminal window")

6. Now, change the directory to the cloned project by entering the following at the prompt:

    ```sh
    cd mcw-oss-paas-devops
    ```

    ![The command to change the directory is displayed in the bash terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image38.png "bash terminal window")

7. Finally, issue a command to open the starter project in VS Code by typing:

    ```sh
    code .
    ```

8. A new VS Code window will open, with the `mcw-oss-paas-devops` folder opened

    ![The MCW-OSS-PAAS-DEVOPS folder is open in the Explorer pane in the Visual Studio Code window, and the Welcome pane is open on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image39.png "Visual Studio Code window")

9. You are now ready to begin working with the project in VS Code

### Task 5: Launch the starter application

In this task, you will seed the MongoDB with sample data, then run the application locally, connected to your MongoDB instance. This task is to verify the connection to MongoDB and that it contains the seeded plan data, before we migrate the application and data to Azure Cosmos DB.

1. Return to VS Code, select **View** from the menu, and select **Integrated Terminal**

    ![Integrated Terminal is highlighted in the View menu (also highlighted) in Visual Studio Code.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image40.png "Visual Studio Code window")

2. This will open a new bash terminal window at the bottom of the VS Code dialog

3. At the bash prompt, use the `npm install` command to ensure the required components are installed on your Lab VM

    ```sh
    npm install
    ```

    ![This is a screenshot of the terminal window at the bottom of the Visual Studio Code dialog box. The command above is entered at the command prompt, and the results of the command are displayed.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image41.png "Bash terminal window")

4. Next, enter the following to seed the local MongoDB database with plans, user accounts, and orders

    ```sh
    node data/Seed.js
    ```

    ![The above command to seed the local MongoDB database is displayed in the bash terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image42.png "Bash terminal window")

    > Note: If you receive an error that the service is not running, start the MongoDB service by executing the `sudo service mongod start` command at the shell prompt.

5. Next, build the application using the following:

    ```sh
    npm run build
    ```

6. Finally, enter the following to start the web server for the application

    ```sh
    npm start
    ```

7. Open a browser and navigate to <http://localhost:3000> to view the landing page of the starter application. You will see three plans listed on the application home page, which are pulled from the local MongoDB database.

    ![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the application home page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image43.png "View the three plans")

8. Return to the VS Code integrated terminal window, and press **CTRL+C** to stop the application

## Exercise 2: Migrate the database to Cosmos DB

Duration: 30 minutes

In this exercise, you will provision an Azure Cosmos DB account, and then update the starter application's database connection string to point to your new Azure Cosmos DB account. You will then, use `mongoimport.exe` to migrate the data in your MongoDB database into Cosmos DB collections, and verify with the application that you are connected to your Cosmos DB database.

### Task 1: Provision Cosmos DB using the MongoDB API

In this task, you will provision a new Azure Cosmos DB account using the MongoDB API.

1. In the Azure portal, select **+Create a resource**, **Databases**, the select **Azure Cosmos DB**

    ![+ Create a resource is highlighted in the navigation pane of the Azure portal, and Databases and Azure Cosmos DB are highlighted on the New blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image44.png "Azure Portal")

2. On the **Azure Cosmos** **DB** blade, enter the following:

    - **ID**: Enter "best-for-you-db-SUFFIX," where SUFFIX is your Microsoft alias, initials, or another value to ensure the name is unique (indicated by a green check mark)

    - **API:** Select **MongoDB**

    - **Subscription:** Select the subscription you are using for this hands-on lab

    - **Resource Group:** Select **Use existing** and choose the **hands-on-lab-SUFFIX resource group** you created previously

    - **Location:** Select a location near you from the list (Note: not all locations are available for Cosmos DB)

    - **Enable geo-redundancy:** Unchecked

    - Select **Create** to provision the new Azure Cosmos DB account

    ![The information above is entered in the Azure Cosmos DB blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image45.png "Azure Cosmos DB")

### Task 2: Update database connection string

In this task, you will retrieve the connection string for your Azure Cosmos DB database and update the starter application's database connection string.

1. When your Cosmos DB account is provisioned, navigate to it in the Azure portal

2. On the **Azure Cosmos DB account** blade, select **Connection String** under **SETTINGS** in the left-hand menu, and copy the **PRIMARY connection string**

    ![Connection String is selected under Settings on the Azure Cosmos DB account blade, and the Primary Connection String value is highlighted on the Read-write Keys tab on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image46.png "Azure cosmos DB account blade")

3. Return to VS Code

4. Open `app.js` from the root directory of the application and locate the line that starts with var **databaseUrl**

    ![App.js is selected and highlighted in the Explorer pane in the Visual Studio Code window, and the line starting with var databaseUrl is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image47.png "Visual Studio Code window")

5. Replace the value of the **databaseUrl** variable with the Cosmos DB connection string you copied from the Azure portal

    ![The value of the databaseUrl variable is displayed in the Visual Studio Code window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image48.png "Visual Studio Code window")

6. Scroll to the end of the value you just pasted in and locate **10255/?ssl=** in the string

    ![10255/?ssl= is highlighted in the string in the Visual Studio Code window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image49.png "Visual Studio Code window")

7. Between the "/" and the "?" insert **best-for-you-organics** to specify the database name

    ![best-for-you-organics is selected in the string in the Visual Studio Code window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image50.png "Visual Studio Code window")

8. Save `app.js`

9. In the VS Code integrated terminal, enter the following command to rebuild the application

    ```sh
    npm run build
    ```

10. When the build completes, start the application by typing the following:

    ```sh
    npm start
    ```

11. Return to your browser and refresh the application page. Note: You may need to press **CTRL+F5** in your browser window to clear the browser cache while refreshing the page.

    ![This is a screenshot of the refreshed application page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image51.png "Refreshed application screenshot")

12. Notice the three plans that were displayed on the page previously are no longer there. This is because the application is now pointed to your Azure Cosmos DB, and there is no plan data in that database yet.

13. Let's move on to copying the data from the local MongoDB instance into Cosmos DB

### Task 3: Pre-create and scale collections

In this task, you will create the collections needed for your database migration and increase each collection's throughput from the default 1,000 RUs to 2,500 RUs. This is done to avoid throttling during the migration, and reduce the time required to import data.

1. In the Azure portal, navigate to your Azure Cosmos DB account, select **Browse** from the left-hand menu, under **COLLECTIONS**, and select **+Add Collection**

    ![Browse is selected and highlighted in the left-hand menu of your Azure Cosmos DB account, and + Add Collection is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image52.png "Azure Cosmos DB account blade")

2. In the **Add Collection** dialog, enter the following:

    - **Database id:** Select **Create new**, and enter "best-for-you-organics"

    - **Collection Id:** Enter "orders"

    - **Storage capacity:** Select **Fixed (10 GB)**

    - **Throughput:** Enter "2500"

    - Select **OK** to create the collection

        ![The information above is entered in the Add Collection dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image53.png "Add Collection dialog box")

3. If they do not already exist, repeat steps 1 and 2 to create collections for:

    - users
    - plans

4. If the users and plans collections already exist from your application connecting, edit each collection to increase their throughput using the following steps

    - Expand the existing collection and select **Scale & Settings**

    - Enter "2500" into the **Throughput** box and select **Save**

    ![The turned delta next to users is highlighted under Collections, Scale & Settings is selected and highlighted below it, and Save is highlighted on the Scale & Settings tab on the right. 2500 is entered in the Throughput box on the Scale & Settings tab.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image54.png "Collections section")

### Task 4: Import data to the API for MongoDB using mongoimport

In this task, you will use `mongoimport.exe` to import data to your Cosmos DB account. There is a shell script located in the `mcw-oss-paas-devops` solution which handles exporting each collection to a JSON file.

1. Next, you will execute a script file, included in the project files you downloaded, to handle exporting the data out of your MongoDB into JSON files on the local file system. These files will be used for the import into Cosmos DB.

2. On your Lab VM, open a new integrated bash prompt in VS Code by selecting the **+** next to the shell dropdown in the integrated terminal pane

    ![This is a screenshot of the terminal window at the bottom of the Visual Studio Code dialog box. The + button is highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image55.png "Bash terminal window")

3. At the prompt, enter the following command to grant execute permissions on the export script:

    ```sh
    chmod +x ~/mcw-oss-paas-devops/data/mongo-export.sh
    ```

4. Next, run the script by entering the following command:

    ```sh
    ~/mcw-oss-paas-devops/data/mongo-export.sh
    ```

    ![The above command to export data from the local MongoDB database is displayed in the bash terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image56.png "Bash terminal window")

5. The script creates the folder ~/MongoExport, and exports each of the collections in your MongoDB to JSON files

6. You are now ready to import the data into Azure Cosmos DB using `mongoimport.exe`

7. Use the following command template for executing the data import:

    ```sh
    mongoimport --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file ~/MongoExport/<your_collection>.json
    ```

8. To get the values needed for the template command above, return to the **Connection string** blade for your Azure Cosmos DB account in the Azure portal. Leave this window up, as will need the **HOST, USERNAME**, and **PRIMARY PASSWORD** values from this page to import data to your Cosmos DB account.

    ![Connection String is selected under Settings on the Azure Cosmos DB account blade. On the Read-write Keys tab on the right, the values under Host, Username, and Primary Password are highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image57.png "Azure Cosmos DB account blade")

9. Replace the values as follows in the template command above:

    - <your_hostname>: Copy and paste the **Host** value from your **Cosmos DB Connection String** blade
    - <your_username>: Copy and paste the **Username** value from your **Cosmos DB Connection String** blade
    - <your_password>: Copy and paste the **Primary Password** value from your **Cosmos DB Connection** **String** blade
    - <your_database>: Enter "best-for-you-organics"
    - <your_collection>: Enter "plans" (Note there are two instances of <your-collection> in the template command)

10. Your final command should look something like:

    ```sh
    mongoimport --host best-for-you-db.documents.azure.com:10255 -u best-for-you-db -p miZiDmNrn8TnSAufBvTQsghbYPiQOY69hIHgFhSn7Gf10cvbRLXvqxaherSKY6vQTDrvHHqYyICP4OcLncqWew== --db best-for-you-organics --collection plans --ssl --sslAllowInvalidCertificates --type json --file ~/MongoExport/plans.json
    ```

11. Copy and paste the final command at the command prompt to import the plans collection into Azure Cosmos DB

    ![The final command to import the plans collection into Azure Cosmos DB is displayed in the Command Prompt window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image58.png "Bash terminal window")

12. You will see a message indicating the number of documents imported, which should be 3 for plans

13. Verify the import by selecting **Data Explorer** in your Cosmos DB account in the Azure portal, expanding plans, and selecting **Documents**. You will see the three documents imported listed.

    ![Data Explorer is selected and highlighted on the Azure Cosmos DB account blade. On the Collections blade, the turned delta next to plans is highlighted, and Documents is selected and highlighted below it. On the Mongo Documents tab to the right, the first of three imported documents is selected and highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image59.png "Azure Cosmos DB account blade")

14. Repeat step 8 for the users and orders collections, replacing the **\<your\_collection\>** values with:

    - users

        ```sh
        mongoimport --host best-for-you-db.documents.azure.com:10255 -u best-for-you-db -p miZiDmNrn8TnSAufBvTQsghbYPiQOY69hIHgFhSn7Gf10cvbRLXvqxaherSKY6vQTDrvHHqYyICP4OcLncqWew== --db best-for-you-organics --collection users --ssl --sslAllowInvalidCertificates --type json --file ~/MongoExport/users.json
        ```

    - orders

        ```sh
        mongoimport --host best-for-you-db.documents.azure.com:10255 -u best-for-you-db -p miZiDmNrn8TnSAufBvTQsghbYPiQOY69hIHgFhSn7Gf10cvbRLXvqxaherSKY6vQTDrvHHqYyICP4OcLncqWew== --db best-for-you-organics --collection orders --ssl --sslAllowInvalidCertificates --type json --file ~/MongoExport/orders.json
        ```

15. To verify the starter application is now pulling properly from Azure Cosmos DB, return to your browser running the starter application (<http://localhost:3000>), and refresh the page. You should now see the three plans appear again on the home page. These were pulled from your Azure Cosmos DB database.

    ![Two Person Plan, High-Pro Plan, and Four Person Plan boxes are visible in this screenshot of the starter application.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image60.png "View the starter application")

16. You have successfully migrated the application and data to use Azure Cosmos DB with MongoDB APIs

17. Return to the Integrated terminal window of VS Code which is running the application, and press **CTRL+C** to stop the application

### Task 5: Install Azure Cosmos DB extension for VS Code

In this task, you will install the Azure Cosmos DB extension for VS Code, to take advantage of the integration with Azure Cosmos DB. This extension allows you to view and interact with your Cosmos DB databases, collections, and documents directly from VS Code.

1. Select the **Extensions** icon, enter "cosmos" into the search box, select the **Azure Cosmos DB** extension, and then select **Install** in the Extension: Azure Cosmos DB window

    ![The Extensions icon is highlighted on the left side of the Extension: Azure Cosmos DB window. On the right, azure cosmos db is in the search box, Azure Cosmos DB is highlighted below it, and Install is highlighted on the right side.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image61.png "Install the Azure Cosmos DB extension")

2. Once the extension installation completes, restart VS Code, and reopen the `mcw-oss-paas-devops` project folder

3. At the bottom left-hand corner of VS Code, you will now see **AZURE COSMOS DB**. Expand that, and select **Sign in to Azure**.

    ![Sign in to Azure is highlighted below AZURE COSMOS DB in the bottom left-hand corner of Visual Studio Code window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image62.png "Sign in to Azure")

4. An info banner will pop up at the top of your VS Code window. Copy the code provided and select **Open**.

    ![The authentication code is highlighted and Open is selected on the right side of the pop-up info banner in the Visual Studio Code window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image63.png "Info banner")

5. In the browser window that opens, paste the copied code into the **Code** box, and select **Continue**

    ![Code is entered in the Code box in the Device Login window, and Continue is selected at the bottom.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image64.png "Device Login window")

6. On the following screens, log in with your Azure account credentials

7. Once you have signed in, you will be taken to a new page in the browser. You can close the browser window.

    ![This is a screenshot of a window indicating that you have signed in, which you can close.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image65.png "Signed iin successfully window")

8. If presented with a prompt to enter a password for a new keyring, enter "**Password.1!!**" as the password, and select **Continue**

    ![Choose password for new keyring dialog, with Password.1!! entered in the password and confirm boxes.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image66.png "Choose password window")

9. Back in VS Code, you should now see your Azure account listed under Azure Cosmos DB, along with your Azure account email listed in the status bar of VS Code

    ![Your Azure account is listed under Azure Cosmos DB, and your Azure email is listed in the status bar of the Visual Studio Code window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image67.png "Azure Cosmos DB window")

10. From here, you can view your databases, collections, and documents, as well as edit documents directly in VS Code, and push the updated documents back into your database

### Task 6: Decrease collection throughput

In this task, you will decrease the throughput on your collections. Azure Cosmos DB uses an hourly billing rate, so reducing the throughput after the data migration will help save costs.

1. In the Azure portal, navigate to your Azure Cosmos DB account, select **Scale** from the left-hand menu, under **COLLECTIONS**

2. Expand the **users** collection and select **Scale & Settings**

3. Change the **Throughput** value to "500," and select **Save**

    ![Scale is selected and highlighted on the left side of your Azure Cosmos DB account. On the Collections blade, the turned delta next to users is highlighted, and Scale & Settings is selected and highlighted below it. On the Scale & Settings tab to the right, 500 is entered and highlighted in the Throughput box, and the Save icon is highlighted above it.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image68.png "Azure Cosmos DB account blade")

4. Repeat steps 2 & 3 for the **plans** and **orders** collections

## Exercise 3: Containerize the app

Duration: 30 minutes

This exercise walks you through containerizing an existing MERN application using Docker, pushing the image to an **Azure Container Registry**, then deploying the image to **Web App for Containers** directly from VS Code.

### Task 1: Create an Azure Container Registry

In this task, you will be creating a private Docker registry in the Azure portal, so you have a place to store the custom Docker image you will create in later steps.

1. In the Azure portal, select **+Create a resource**, **Containers**, and select **Azure Container Registry**

    ![+ Create a resource is highlighted in the navigation pane of the Azure portal, Containers is selected and highlighted in the middle, and Azure Container Registry is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image69.png "Azure Portal")

2. On the **Create container registry** blade, enter the following:

    - **Registry name:** Enter "bestforyouregistrySUFFIX," where SUFFIX is your Microsoft alias, initials, or another value to ensure the name is unique (indicated by a green check mark)

    - **Subscription:** Select the subscription you are using for this hands-on lab

    - **Resource group:** Select **Use existing** and select the **hands-on-lab-SUFFIX** resource group created previously

    - **Location:** Select the location you are using for resources in this hands-on lab

    - **Admin user:** Select **Enable**

    - **SKU:** Select **Basic**

    - Select **Create** to provision the new Azure Container Registry

        ![The information above is entered on the Create container registry blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image70.png "Create container registry blade")

### Task 2: Install Docker extension in VS Code

The Docker extension for VS Code is used to simplify the management of local Docker images and commands, as well as the deployment of a built app image to Azure.

1. On your Lab VM, return to VS Code, and the open starter project

2. Select the **Extensions icon** from the left-hand menu, enter "Docker" into the search box, select the **Docker** extension, and in the **Extension: Docker** window, select **Install**

    ![Extensions is highlighted in the Visual Studio Code navigation pane, docker is entered in the Extensions search box, Docker is highlighted below, and Install is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image71.png "Visual Studio Code window")

3. Close and reopen VS Code, and the `mcw-oss-paas-devops` project

4. Expand the **DOCKER** extension block, then expand **Registries** and **Azure**, and you should see your Azure Container Registry listed. You should already be logged into your Azure subscription, but if not, follow the steps in [Exercise 2, Task 5 Step 3](#task-5-install-azure-cosmos-db-extension-for-vs-code) to sign in.

    ![The Azure Container Registry is selected under Registries and Azure in the DOCKER extension block.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image72.png "Docker extension block")

### Task 3: Create Docker image and run the app

In this task, you will use VS Code, and the Docker extension, to add the necessary files to the project to create a custom Docker image for the `mcw-oss-paas-devops` app.

1. On your Lab VM, return to VS Code, and the `mcw-oss-paas-devops` project

2. Open the VS Code Command Palette, by selecting **View** from the menu, then **Command Palette**

3. Enter "add docker" into the Command Palette and select **Docker: Add docker files to workspace**

    ![Docker: Add docker files to workspace is selected under add docker in the Command Palette.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image73.png "Select Docker: Add docker files to workspace")

4. At the **Select Application Platform** prompt, select **Node.js**

    ![Node.js is highlighted at the Select Application Platform prompt.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image74.png "Select Node.js")

5. Ensure port "3000" is entered on the next screen, and press **Enter**

    ![3000 is selected in the field above What port does your app listen on?](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image75.png "Explorer")

6. This will add Dockerfile, along with several configuration files for Docker compose to the project

    ![Dockerfile and several configuration files are highlighted in the Explorer pane.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image76.png "Explorer pane")

7. Select **Dockerfile** from the file navigator and observe the contents. This file provides the commands required to assemble a Docker image for the `mcw-oss-paas-devops` application.

    ![This is a screenshot of the Dockerfile contents. At this time, we are unable to capture all of the information in the window. Future versions of this course should address this.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image77.png "Dockerfile screenshot")

8. Next, you will tell Docker to build and image for your app. Open the **VS Code Command Palette** again and run **Docker: Build Image** to build the image you just created

    ![Docker: Build Image is highlighted in the Command Palette.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image78.png "Run Docker: Build Image")

9. In the **Choose Dockerfile to build** box, select **Dockerfile** (the Dockerfile that was just created)

    ![Dockerfile is highlighted in the Choose Dockerfile to build box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image79.png "Select Dockerfile")

10. In the next box, you will provide a **registry**, **image name**, and **tag**, using the following format. This format will allow the image to be pushed to your container registry.

    > [registry]/[image name]:[tag]

11. For this, you will need the Login server value from your Azure Container registry's **Access keys** blade

    ![Access keys is selected under Settings on the Azure Container registry's Access keys blade, and on the right, bestforyouregistry.azurecr.io is highlighted in the Login server box on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image80.png "Container registry blade")

12. Entry the following into the **Tag image as...** box, where [Login server] is the Login server value from Azure:

    ```sh
    [Login server]/best-for-you-organics:latest
    ```

13. For example:

    ```sh
    bestforyouregistry.azurecr.io/best-for-you-organics:latest
    ```

14. Enter your value, and press **Enter**, which will trigger the build of the image

    ![The Tag image as value is highlighted in the Command Palette.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image81.png "Enter the Tag image as value")

15. In the terminal window, you will see the following docker build commands execute:

    ![This is a screenshot of Docker build command's output in the terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image82.png "Terminal window")

16. Once the build completes, you will see the image show up in the **DOCKER** extension explorer, under **Images**

    ![The image is highlighted in the DOCKER extension explorer under Images.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image83.png "Docker extension explorer")

17. You can also use the docker images command in the Integrated terminal to list the images

### Task 4: Run the containerized App

In this task, you will run the app from the container you built in the previous task.

1. In the **Images** area of the Docker extension in VS Code, right-click your image, and select **Run Interactive**

    ![The image is selected in the Images area of the Docker extension explorer, and Run Interactive is highlighted in the shortcut menu.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image84.png "Docker extension explorer")

2. Notice in the Interactive terminal that a docker run command is issued. Using the VS Code Docker extension, you can issue some docker commands, without needing to work from the command line.

    ![The docker run command is highlighted in the Interactive terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image85.png "Interactive terminal window")

3. Verify the web app and container are functioning by opening a browser window and navigating to <http://localhost:3000>

    ![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the web app, and localhost:3000/ is highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image86.png "View the web app")

4. In the Integrated terminal of VS Code, for the interactive session, press **CTRL+C** to stop the container

### Task 5: Push image to Azure Container Registry

In this task, you are going to push the image to your Azure Container Registry.

1. In the Azure portal, navigate to the hands-on-lab-SUFFIX resource group, and select the **bestforyouregistrySUFFIX** Container registry from the list of resources

    ![The bestforyouregistry Container registry is highlighted in the list of resources on the Azure portal.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image87.png "List of resources")

2. On the **bestforyouregistrySUFFIX** blade, select **Access keys** under settings in the left-hand menu, and leave this page up as you will be referencing the **Login server**, **Username**, and **password** in the next task

    ![Access keys is selected under Settings on the Azure Container registry's Access keys blade, and the values in the Login server, Username, and Name boxes are highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image88.png "Container Registry blade")

3. Return to the Integrated terminal window in VS Code and enter the following command to log in to your Azure Container registry, replacing the bracketed values with those from the container registry access keys page

    ```sh
    docker login [Login Server] -u [Username]
    ```

4. For example:

    ```sh
    docker login bestforyouregistry.azurecr.io -u bestforyouregistry
    ```

5. Enter the password when prompted to complete the login process

    ![This is a screenshot of the password prompt in the Visual Studio Code Integrated terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image89.png "Integrated terminal window")

6. Once you are successfully logged in, find your image in the Docker extension section of the VS Code, right-click your image, and select **Push**

    ![The image is selected in the Images area of the Docker extension explorer, and Push is highlighted in the shortcut menu.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image90.png "Docker extension explorer")

7. Note the "docker push" command issued in the terminal window

    ![This is a screenshot of the docker push command in the Visual Studio Code Integrated terminal window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image91.png "Integrated terminal window")

8. To verify the push, return to the **bestforyouregistry** blade in the Azure portal, and select **Repositories** under **SERVICES** on the left-hand side, and note the **best-for-you-organics** repository

    ![In your Azure Container Registry's repository, Repositories is selected and highlighted under Services, and best-for-you-organics is highlighted under Repositories.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image92.png "Container registry blade")

## Exercise 4: Set up Web App for Containers

Duration: 10 minutes

In this exercise, you will deploy the containerized app to a Web App for Containers instance from the image stored in your Azure Container Registry.

### Task 1: Provision Web App for Containers

1. In the Azure portal, select **+Create a resource**, **Web + Mobile**, and select **Web App for Containers**

    ![+ Create a resource is highlighted in the navigation pane of the Azure portal, Web + Mobile is highlighted in the middle, and Web App for Containers is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image93.png "Azure Portal")

2. On the **Create** blade, enter the following:

    - **App name**: Enter "best-for-you-app-SUFFIX" (the name must be unique)

    - **Subscription**: Select the subscription you are using for this lab

    - **Resource group**: Select **Use existing** and choose the hands-on-lab-SUFFIX resource group

    - **App Service plan/Location**: Accept the default assigned value, which will create a new App Service plan

    - Select **Configure container**, and enter the following:

        - **Image source:** Select **Azure Container Registry**

        - **Registry**: Select **bestforyouregistrySUFFIX**

        - **Image**: Select **best-for-you-organics**

        - **Tag**: Select **latest**

        - **Startup File**: Leave blank

        - Select **OK**

    - Select **Create**

        ![The information above is entered on the Create container registry blade, and Configure container is highlighted on the left side. On the right, Azure Container Registry is highlighted under Image source, and more information from above is entered.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image94.png "Web App for Containers and Docker Container blades")

### Task 2: Navigate to the deployed app

In this task, you will navigate to the deployed app, and log in to verify it is functioning correctly.

1. When you receive the notification that the Web App for Containers deployment has completed, navigate to the Web App by selecting the **notifications icon**, and selecting **Go to resource**

    ![The Go to resource button is highlighted at the bottom of the successful deployment notification window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image95.png "Notifications window")

2. On the **Overview** blade of **App Service**, select the URL for the App Service

    ![The URL for the App Service is highlighted on the Overview blade of App Service.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image96.png "App Service blade")

3. A new browser window or tab will open, and you should see the `mcw-oss-paas-devops` application's home page displayed

4. Sign in to the site with the following credentials to verify everything is working as expected

    - <demouser@bfyo.com>
    - Password.1!!

        ![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the mcw-oss-paas-devops home page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image97.png "Sign in to the mcw-oss-paas-devops home page")

## Exercise 5: Configure CI/CD pipeline

Duration: 60 minutes

In this exercise, you are going to use Jenkins to implement a continuous integration (CI) and continuous delivery (CD) pipeline to deploy the containerized MERN app to Web App for Containers.

### Task 1: Prepare GitHub account for service integrations

In this task, you will be adding a Jenkins service integration into your GitHub account. This integration will enable a Jenkins CI build job to be triggered when code is checked in to your GitHub repository.

1. On your Lab VM, navigate to your Jenkins VM in the [Azure portal](https://portal.azure.com/)

    - Select **Resource groups** from the left-hand menu, then enter "Jenkins" into the search box, and select your **Jenkins-SUFFIX** resource group from the list

    ![Resource groups is highlighted in the navigation pane of the Azure portal, Jenkins is highlighted in the search box, and jenkins is highlighted in the search results.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image98.png "Azure Portal")

    - On the Jenkins-SUFFIX Resource group blade, select your **Jenkins** virtual machine

    ![Overview is highlighted in the jenkins resource group blade, and Jenkins is highlighted in the Filter by name results.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image99.png "Resource group blade")

2. On the **Overview** blade of your Jenkins virtual machine, locate the **DNS name**, and copy the value

    ![DNS name and its value are highlighted on the Overview blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image100.png "Overview blade")

3. Return to your forked `mcw-oss-paas-devops` application page in your GitHub account, select **Settings**, then select **Integrations & services** from the left-hand menu

    ![Integrations & services is highlighted on the left-hand menu, and Settings is highlighted at the top.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image101.png "Select Settings")

4. Select **Add service**, enter "Jenkins" into the search filter, and select **Jenkins (GitHub plugin)**

    ![The Add service button is highlighted under Installed GitHub Apps, Jenkins (gith is highlighted in the Available Services search box, and Jenkins (GitHub plugin) is highlighted in the search results.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image102.png "Add the Jenkins GitHub plugin")

5. When prompted, enter your GitHub account password to continue

    ![A password is entered in the Confirm password to continue dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image103.png "Confirm password dialog box")

6. In the Jenkins hook URL text box, enter "http://YOUR-JENKINS-URL]/github-webhook/" replacing **[YOUR-JENKINS-URL]** with the Jenkins DNS name you copied from the Azure portal, and select **Add service**

    ![The value in the Jenkins hook url box is highlighted in the Services / Add Jenkins (GitHub plugin) dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image104.png "Select Add service")

7. You will see the Jenkins (GitHub plugin) under the list of services

    ![Jenkins (GitHub plugin) is highlighted in the list of services.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image105.png "Installed GitHub Apps section")

8. Next, you need to grant the Jenkins user access to your GitHub repository by adding a deploy key in the GitHub settings

9. Return to your Jenkins virtual machine page in the Azure portal, select **Connect**, and copy the SSH command

    ![The Connect icon is highlighted on the Azure portal, and the SSH command is highlighted below.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image106.png "Jenkins virtual machine page")

10. Open a new bash shell and paste the SSH command you copied above at the prompt. Enter "yes" if prompted about continuing to connect, and enter the jenkinsadmin password, "Password.1!!," when prompted.

    ![The information above is displayed in this screenshot of the bash terminal.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image107.png "bash terminal screenshot")

11. At the jenkinsadmin\@Jenkins prompt, enter:

    ```sh
    ssh-keygen
    ```

12. Press **Enter** to accept the default file in which to save the key

13. Press **Enter** to use an empty passphrase, and re-enter it to confirm. (Note: This is done only for simplicity in this hands-on lab, and is not recommend for actual environments.)

14. Copy the location into which your public key has been saved

    ![In this screenshot of the bash terminal, ssh-keygen and the location into which your public key has been saved are highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image108.png "bash terminal screenshot")

15. Show the public key using the following command, replacing [KEY-PATH] with the location of your public key.

    ```sh
    cat [KEY-PATH]
    ```

16. Copy the key displayed, so it can be added to GitHub

    ![The displayed key is highlighted in this screenshot of the bash terminal.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image109.png "bash terminal screenshot")

17. Return to you GitHub account in the browser, select the **Deploy keys** option from the left-hand menu, and select **Add deploy key**

    ![Deploy keys banner is displayed, and the Add deploy key button is highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image110.png "Deploy keys")

18. Enter "Jenkins" for the title, paste the SSH key you copied above into the Key field, removing any trailing spaces, and select **Add key**

    ![On the GitHub account page, Deploy keys is selected in the left-hand menu, Jenkins is in the Title box, and the SSH key that you copied is in the Key field.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image111.png "Add the key")

19. To ensure that everything is working, return to the Jenkin's bash shell, and enter the below command which will check the connection to GitHub

    ```sh
    ssh git@github.com
    ```

20. Enter "yes" when prompted about continuing

21. You should see a message like the following, indicating a successful connection

    ![A message indicating a successful connection is highlighted in this screenshot of the Jenkins bash terminal.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image112.png "bash terminal")

22. The GitHub side of the integration with Jenkins is complete. Next, you will configure Jenkins as part of your CI/CD pipeline.

### Task 2: Open connection to Jenkins

In this task, you will create an SSH tunnel to the Jenkins server, and configure the Jenkins server for use with the MERN application.

1. Return to your **Jenkins** VM blade in the Azure portal

2. On the **Overview** blade of your Jenkins VM, locate the **DNS name**, and copy the value

    ![DNS name and its value are highlighted on the Overview blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image100.png "Overview blade")

3. Open a new browser window or tab and paste the copied DNS name into the browser's address bar to navigate to your Jenkins server

4. On the Jenkins on Azure screen, you will see a message that this Jenkins instance does not support https, so logging in through a public IP address has been disabled. You will need to create an SSH tunnel to securely connect to the Jenkins instance.

5. To set up an SSH tunnel to Jenkins, copy the ssh command provided in the Jenkins on Azure window, as highlighted in the screen shot below

    ![The ssh command that Jenkins provides is highlighted in the Jenkins on Azure window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image113.png "Jenkins On Azure window")

6. Open a new bash shell, and at the command prompt paste the copied ssh command, replacing "username" with **jenkinsadmin**. The command will resemble the following:

    ```sh
    ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins-kb.westus.cloudapp.azure.com
    ```

7. If prompted that authenticity of the Jenkins host cannot be established, enter "yes" to continue

8. Enter the **jenkinsadmin** password, "Password.1!!"

    ![The ssh command above is highlighted in the bash window, and yes is highlighted next to Are you sure you want to continue connecting (yes/no)?](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image114.png "bash window")

9. After you have started the SSH tunnel, open a new browser tab or window, and navigate to <http://localhost:8080/>

    ![After navigating to http://localhost:8080, the Getting Started page is displayed, providing the path to the initial administrator password, /var/lib/jenkins/secrets/initialAdminPassword.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image115.png "Unlock Jenkins window")

10. To get the initial password, copy the path provided, return to the SSH tunnel bash window, and run the following command:

    ```sh
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

11. Copy the password returned

    ![The returned password is highlighted in the bash window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image116.png "bash window")

12. Return to the Getting Started screen in your browser, paste the password into the **Administrator password** box, and select **Continue**

    ![Screenshot of the Administrator password pasted into the box on the Getting Started screen, and Continue selected.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image117.png "Administrator password field")

13. On the Customize Jenkins screen, select **Install suggested plugins**

    ![Screenshot of the Customize Jenkins page, with Install suggested plugins highlighted and selected.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image118.png "Customize Jenkins page")

14. On the Create First Admin User screen, enter the following:

    - **Username**: Enter a username, such as your first name

    - **Password**: Password.1!!

    - **Confirm Password**: Password.1!!

    - **Full name**: Enter your first name

    - **E-mail address**: Enter your email address

    - Select **Save and Finish**

        ![The Create First Admin User page, with the values specified above entered into the appropriate fields, and Save and Finish highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image119.png "Create First Admin User page")

15. Select **Start using Jenkins** on the Jenkins is ready screen

    ![Screenshot of the Jenkins is ready page, with the Start using Jenkins button highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image120.png "Jenkins is ready page")

16. You will be redirected to the Jenkins dashboard

    ![Screenshot of the Jenkins dashboard.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image121.png "Jenkins dashboard")

17. From the Jenkins dashboard, select **Manage Jenkins** from the left-hand menu and then select **Manage Plugins**

    ![Manage Jenkins is highlighted in the left-hand menu of the Jenkins window, and Manage Plugins is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image122.png "Jenkins window")

18. Install the **NodeJS** plug-in by entering **nodejs** into the Filter box, and selecting the **NodeJS** plug-in in the results, and then selecting **Install without restart**

    ![On the Manage Plugins screen, the Available tab is selected, \"nodejs\" is entered into the filter box, and NodeJS is checked in the filter results. The Install without restart button is highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image124.png "Manage Plugins page")

19. Scroll up to the top the screen, and select **Manage Jenkins** from the left-hand menu

    ![Screenshot of the Jenkins left-hand menu with Manage Jenkins link highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image125.png "Jenkins menu")

20. Select **Global Tool Configuration**

    ![Screenshot of Manage Jenkins page, with Global Tool Configuration option highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image126.png "Manage Jenkins page")

21. Find **NodeJS** and select **Add NodeJS** next to NodeJS installations

22. Enter a name, ensure **Install automatically** is checked, and accept the default (latest) version of nodejs

    ![In the NodeJS dialog box, bestforyounode is in the Name box, and Install automatically is selected below it.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image127.png "NodeJS dialog box")

23. Select **Save**

    ![This is a screenshot of the Save (selected) and Apply buttons.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image128.png "Select Save")

### Task 3: Configure Continuous Integration with Jenkins

In this task, you will set up a simple Jenkins continuous integration (CI) pipeline, which will build the `mcw-oss-paas-devops` application with every code commit into GitHub.

1. Return to the **Jenkins** dashboard, and select **New Item** from the left-hand menu

2. Enter "best-for-you-build" as the name, select **Freestyle project**, and select **OK**

    ![Best-for-you-build is entered in the Enter an item name box, and Freestyle project and OK are selected below it.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image129.png "Enter an item name section")

3. On the **General** tab of the project page, select **GitHub project**, and enter the URL for your forked copy of the best-for-you-organics project page in your GitHub account

    ![On the General tab of the project page, GitHub project is selected and the URL for your forked copy of the best-for-you-organics project page in your GitHub account is entered.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image130.png "Project page General tab")

4. Also under the **General** tab, uncheck **Restrict where this project can be run** if it is checked.

5. Next, scroll down to the **Source Code Management** section, select **Git**, and enter the URL to your project, including the ".git" extension

    ![Git is selected on the Source Code Management tab of the project page, and the URL to your project, including the .git extension, is entered in the Repository URL box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image131.png "Source Code Management section")

6. Scroll down to the **Build Triggers** section and select **GitHub hook trigger for GITScm polling**

    ![The GitHub hook trigger for GITScm polling is selected and highlighted in the Build Triggers section.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image132.png "Build Triggers section")

7. Scroll down to the **Build Environment** section and select **Provide Node & npm bin/ folder to PATH**, select your NodeJS Installation from the list, and leave the default value for npmrc file

    ![In the Jenkins Build Environment section, Provide Node & npm /bin folder to PATH is checked, the NodeJS Installation is specified, and the default value is set for npmrc](media/jenkins-build-environment.png "Jenkins Build Environment")

8. In the **Build** section, select **Add build step**, and select **Execute shell** from the options

    ![In the Jenkins Add build step menu, Execute shell is highlighted.](media/jenkins-add-build-step-execute-shell.png "Add Execute Shell Build step")

9. In the **Execute shell** Command block, enter:

    ```sh
    npm install
    npm run build
    ```

    ![In the Execute build section, npm install and npm run build are entered on separate lines in the command block.](media/jenkins-build-execute-shell-command.png "Jenkins Execute Build shell")

10. Finally, select **Save**

    ![This is a screenshot of the Save (selected) and Apply buttons.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image133.png "Select Save")

11. Your Jenkins CI build job should now be triggered whenever a push is made to your repository

### Task 4: Trigger CI build

In this task you will commit your pending changes in VS Code to you GitHub repo, and trigger the Jenkins CI build job.

1. Return to VS Code on your Lab VM, and the open `mcw-oss-paas-devops` project

2. Observe that the **source control icon** on the left-hand navigation bar has a badge indicating you have uncommitted changes. Select the **icon**

    ![The Source Control icon is highlighted on the Visual Studio Code Activity Bar.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image134.png "Visual Studio Code Activity Bar")

3. In the **SOURCE CONTROL: GIT** pane, enter a commit message, such as "Added Docker configuration," and select **+** next to **CHANGES** to stage all the pending changes

    ![Added Docker configuration is highlighted in the SOURCE CONTROL: GIT pane, and the plus sign (+) next to CHANGES is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image135.png "Visual Studio Code Activity Bar")

4. Select the **checkmark** to commit the changes

    ![The check mark next to commit the changes is highlighted in the SOURCE CONTROL: GIT pane.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image136.png "Visual Studio Code Activity Bar")

5. Next, select the **ellipsis** to the right of the check mark, and select **Push** from the dropdown

    ![The ellipsis (...) to the right of the check mark is highlighted in the SOURCE CONTROL: GIT pane, and Push is highlighted in the submenu.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image137.png "Visual Studio Code Activity Bar")

6. If prompted, enter your GitHub account credentials to log into your GitHub account

    > NOTE: You will need to user your GitHub username (not your email address) here, and the password will be the Personal Access Token you created and saved previously.

7. Return to your best-for-you-build job in Jenkins, and locate the **Build History** block on the left-hand side. Select **#1** to view the details of the build job, caused by your GitHub commit

    ![Screenshot of Build History on the best-for-you-organics project page, with build job \#1 highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image138.png "Build History section")

8. On the build page, you can see the changes you committed

    ![The committed changes are displayed on the build page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image139.png "Build page")

9. You have successfully set up your CI pipeline

### Task 5: Install Docker on the Jenkins VM

In this task, you will install Docker CE on your Jenkins VM, so it can be used to build images from the build artifacts produced by your CI build.

1. The first step is to ensure no older versions of Docker are installed on your Jenkins VM. Using the SSH tunnel bash terminal you opened previously, navigate to the command prompt, and enter:

    ```sh
    sudo apt-get remove docker docker-engine docker.io
    ```

2. Next, you need to set up a Docker repository on the host machine. The begin, update the `apt` package index:

    ```sh
    sudo apt-get update
    ```

3. Install the necessary packages to allow `apt` to use a repository over HTTPS, entering `y` when prompted to continue

    ```sh
    sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
    ```

4. Add Docker's official GPG key

    ```sh
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

5. Verify that you now have the key with fingerprint `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, by searching for the last 8 characters of the fingerprint:

    ```sh
    sudo apt-key fingerprint 0EBFCD88
    ```

6. You should see output similar to:

    ```sh
    pub   4096R/0EBFCD88 2017-02-22
        Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
    uid                  Docker Release (CE deb) <docker@docker.com>
    sub   4096R/F273FCD8 2017-02-22
    ```

7. Next, enter the following commands to set up the **stable** repository

    ```sh
    sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
    ```

8. You are now ready to install Docker CE. Start by updating the `apt` package index

    ```sh
    sudo apt-get update
    ```

9. Install the latest version of Docker CE, entering `y` when prompted to continue

    ```sh
    sudo apt-get install docker-ce
    ```

10. Verify that Docker CE is installed correctly by running the `hello-world` image

    ```sh
    sudo docker run hello-world
    ```

11. The final step is to add permission to the `jenkins` user to Docker

    ```sh
    sudo usermod -a -G docker jenkins
    sudo chmod 664 /run/docker.sock
    ```

12. Now, restart the Jenkins service, entering the jenkinsadmin password, Password.1!!, when prompted

    ```sh
    service jenkins restart
    ```

### Task 6: Add an Azure service principal for Jenkins

In this task, you will create an Azure Active Directory (Azure AD) application and service principal (SP) that will provide the Jenkins CD pipeline access to Azure resources. You will grant the SP permissions to the hands-on-lab-SUFFIX resource group.

1. In the [Azure portal](https://portal.azure.com), select **Azure Active Directory** from the navigation menu, and then select **App registrations** and **+New application registration**

    ![Azure Active Directory (Azure AD) is highlighted in the Azure navigation menu, and App registrations is selected and highlighted in the left-hand menu of the Azure AD blade. New application registration is highlighted.](media/azure-active-directory-app-registrations-new.png "New Azure AD application registration")

2. On the Create blade, enter the following:

    - **Name**: Enter best-for-you-app, or something similar

    - **Application type**: Select Web app / API

    - **Sign-on URL**: Enter https://bestforyou.com

        ![In the Create Azure AD App registration blade, the values specified above are entered into the appropriate fields.](media/azure-active-directory-app-registrations-create.png "Create Azure AD App registration")

3. When the app finishes creating, you will be taken to the blade for the app. On the Registered app blade, copy the **Application ID**, and save the value in a text editor for later user. You will need this value in Jenkins to specify the credentials for connecting to Azure.

    ![On the Registered app blade, Application ID is highlighted.](media/registered-app-application-id.png "Registered app")

4. Now, select **Settings** on the Registered app blade, and then select **Keys**

    ![On the Registered app blade, Settings is highlighted.](media/registered-app-blade-settings.png "Registered app")

5. On the Keys blade, create a new Password by entering a **Key description**, selecting a **Duration**, and selecting **Save**

    ![On the Keys blade, a new password description and expiration are entered.](media/registered-app-keys-new-password.png "Registered app Keys")

6. Copy the generated value and save it into a text editor for future reference

    ![On the Keys blade, the generated password value is selected and highlighted.](media/registered-app-keys-generated-password.png "Registered app Keys")

7. Return to the Azure AD blade in the portal and select **Properties** from the left-hand menu, then copy the **Directory ID** value from the Properties blade. Paste this value in a text editor for later reference, as the Tenant ID of your subscription.

    ![On the Azure AD properties blade, Directory ID is highlighted.](media/azure-active-directory-properties.png "Azure AD properties")

8. Next, select **Resource groups** from the Azure navigation menu, enter "hands-on-lab-SUFFIX" into the filter box, and select the hands-on-lab-SUFFIX resource group from the list

9. On your hands-on-lab-SUFFIX blade, copy the **Subscription ID** in the Essentials area of the Overview blade, and paste the value into a text editor for later reference

    ![On the Overview blade of the resource group, Subscription ID is highlighted.](media/resource-group-overview-subscription-id.png "Resource group overview blade")

10. Select **Access control (IAM)** from the left-hand menu of the Resource group blade, and then select **+ Add**

    ![On the Resource group blade, Access control (IAM) is selected, and +Add is highlighted.](media/resource-group-access-control-add.png "Resource group Access control (IAM)")

11. On the Add permissions blade, do the following:

    - **Role**: Select Contributor
    - **Assign access to**: Select Azure AD user, group, or application
    - **Select**: Enter "best" and select the best-for-you-app Registered app you created in Azure AD
    - Select **Save**

        ![In the Add permission dialog, the Role is set to Contributor, Azure AD user, group, or application is selected in the Assign access to drop down, "best" is entered into the Select box, and best-for-you-app is listed under Selected members.](media/resource-group-access-control-add-permission.png "Add permission")

### Task 7: Add continuous delivery to Jenkins build job

In this task, you will use the [Azure App Service Jenkins plugin](https://plugins.jenkins.io/azure-app-service) to add continuous deployment (CD) to the Jenkins build pipeline. This will use a post-build action to create a Docker new image from the build, push that image to your Azure Container Registry, and deploy the image to your Web App for Containers instance. This post-build action will run under the credentials of the SP you created in the previous task.

1. Return to your **Jenkins** dashboard, and select the **best-for-you-build** project.

    ![The best-for-you-build project is highlighted on the Jenkins dashboard.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image149.png "Jenkins dashboard")

2. Select **Configure** from the left-hand menu.

    ![Configure is highlighted in the left-hand menu.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image150.png "Jenkins left-hand menu")

3. On the configure screen, scroll down to Post-build Actions, select **Add post-build action** and then select **Publish an Azure Web App**

    ![In the Jenkins post-build action menu, Publish an Azure Web App is highlighted.](media/jenkins-post-build-action-publish-azure-web-app.png "Jenkins post-build action menu")

4. In the Publish an Azure Web App section that appears, select **Add** next to **Azure Credentials**

    ![In the Publish an Azure Web App post-build action, Add Azure credentials is highlighted.](media/jenkins-post-build-action-publis-azure-web-app-credentials-add.png "Add Azure Credentials")

5. On the Jenkins Credentials Provider screen, enter the following:

    - **Domain**: Leave set to Global credentials (unrestricted)
    - **Kind**: Select Microsoft Azure Service Principal
    - **Scope**: Leave set to Global (Jenkins, nodes, items, all child items, etc)
    - **Subscription ID**: Enter your Azure subscription ID which you copied into a text editor from the Resource group blade
    - **Client ID**: Enter the best-for-you-app Application ID you copied into the text editor
    - **Client Secret**: Enter the password you created on the Keys blade for the best-for-you-app registered app in Azure AD
    - **Tenant ID**: Enter the Directory ID you copied into the text editor from the properties blade of your Azure AD account
    - **Azure Environment**: Leave set to Azure
    - **ID**: Enter jenkinsSp
    - **Description** Leave blank, or enter a description if you like
    - Select **Verify Service Principal** and ensure you see the _Successfully verified the Microsoft Azure Service Principal_ message
    - Select **Add**

        ![On the Jenkins Credentials Provider screen, the values specified above are entered into the appropriate fields.](media/jenkins-credential-provider.png "Jenkins Credentials Provider")

6. Back in the Publish an Azure Web App of the post-build actions tab, select the newly created jenkinsSp credentials in the Azure Credentials list

    ![The newly created jenkinsSp credentials are selected in the Azure Credentials box of the Azure Profile Configuration section.](media/jenkins-azure-profile-configuration.png "Azure Profile Configuration")

7. In the App configuration section:

    - **Resource Group Name**: Select the hands-on-lab-SUFFIX resource group
    - **App Name**: Select best-for-you-app
    - Select **Publish via Docker**
    - **Dockerfile path**: Leave set to **/Dockerfile
    - **Docker registry URL**: Enter your Azure Container Registry's Login server value, which you can retrieve from the Access keys blade in the Azure portal
    - **Registry credentials**: Select **Add**, and in the add dialog enter the following:
        - **Domain**: Leave set to Global credentials (unrestricted)
        - **Kind**: Set to Username with password
        - **Scope**: Leave set to Global (Jenkins, nodes, items, all child items, etc)
        - **Username**: Enter the Username value for your Azure Container Registry, which you can retrieve from the Access keys blade of your ACR in the Azure portal
        - **Password**: Enter the password value for your Azure Container Registry, which you can retrieve from the Access keys blade of your ACR in the Azure portal

            ![In the Container Registry Access keys blade, ](media/azure-container-registry-access-keys.png "Container Registry Access keys")

        - **ID**: Enter acrCreds
        - **Description**: Leave blank, or enter a description if you like
        - Select **Add**

            ![In the Jenkins Credential Provider dialog, the values specified above are entered into the appropriate fields.](media/jenkins-credentials-provider-username-with-password.png "Add credentials")

    - Select the newly added acrCreds credentials from the Registry credentials list (will be listed as [the name of your registry]/******)
    - Select the **Advanced** button
    - **Docker Image**: Leave blank
    - **Docker Image Tag**: Leave blank
    - Select **Verify Docker Configuration** and ensure the _Docker registry configuration verified_ message is displayed

        ![In the Publish an Azure Web App post-build action section, the values specified above are entered into the appropriate fields.](media/jenkins-publish-an-azure-web-app.png "Publish an Azure Web App post-build action")

8. Select **Save**

### Task 8: Trigger CI-CD pipeline

In this task, you will commit changes to the `mcw-oss-paas-devops` starter application and trigger the full CI/CD pipeline through Jenkins, resulting in the updated application being added to a new Docker image, pushed to ACR, and deployed to Web App for Containers.

1. Return to VS Code on your Lab VM, open the `.dockerignore` file, and delete the line containing __DockerFile*__ from the file. This will allow Jenkins to use the file to build the container in the Jenkins CI/CD pipeline.

2. Next, open the `src/components/plan/Plans.js` file, and insert the following markup between `<div class="container">` and `<Grid>`

    ```html
    <h3>Welcome to Best For You Organics Company</h3>
    ```

    ![The src/components/plan/Plans.js file is highlighted on the left side of Visual Studio Code, and the markup above is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image174.png "VS Code")

3. Save the updated files

4. As you did in [Task 4](#task-4-trigger-ci-build), above, select the **Source Control icon** from the left-hand menu, enter a commit comment, select **+** to stage the change, and select the **checkmark** to commit the change, and push to GitHub. Enter your credentials if prompted. This will trigger the Jenkins CI/CD pipeline.

    ![The updated files are listed under Staged Changes in VS Code, and a commit comment is entered.](media/visual-studio-code-cd-file-updates-commit.png "Visual Studio Code")

5. Return to your Jenkins dashboard, and select the **best-for-you-build** project, and select the latest build number under Build History

    ![The Jenkins build history is displayed, with a progress bar next to #2](media/jenkins-build-history.png "Jenkins build history")

6. On the Build page, select **Console Output** from the left-hand menu

    ![Console Output is highlighted in the left-hand menu on the Jenkins project build page](media/jenkins-build-console-ouptput.png "Build console output")

7. On the Console Output page, you can monitor the build progress. When it completes after a few minutes, you will see a SUCCESS message similar to the following:

    ![A build and Docker deployment success message is displayed in the console output in Jenkins](media/jenkins-build-console-output-success.png "Build and Docker deployment success")

8. Once the deployment is complete, you can verify the changes deployed successfully by going to Azure and reloading the web page for your App Service in the browser. The deployment of the container can take several minutes to complete so refreshes may take a few minutes to show the new header.

    > TIP: It may help to open the app in an Incognito or InPrivate browser window, so you don't have the old page cached

9. When the deployment is complete you should see the home page, with a new header above the three plans on the page

    ![Welcome to Best for You Organics Company! is highlighted above the Two Person Plan, Four Person Plan, and High-Pro Plan boxes in this screenshot of the home page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image179.png "Home page")

## Exercise 6: Create Azure Function for order processing

Duration: 45 minutes

In this task, you will create an Azure Function that will be triggered by orders being added to the Orders collection in Azure Cosmos DB. This Function will trigger whenever a document in the orders collection is inserted or updated. The function checks the processed field on the order document, ensuring only unprocessed orders are sent to the processing queue.

### Task 1: Provision a Function App

In this task, you will create a Function App in Azure, which will host your Functions

1. In the Azure portal, select **+Create a resource**, enter "function app" in to the **Search the marketplace** box, and select **Function App** from the results

    ![+ Create a resource is highlighted in the navigation pane of the Azure portal, and Everything is selected and highlighted in the middle under Marketplace. On the right, function app is highlighted in the search box, and the Function App row is highlighted in the results below that.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image180.png "Azure Portal")

2. On the **Function App** blade, select **Create**

3. On the **Create Function App** blade, enter the following:

    - **App name:** Enter a unique name, such as "bestforyouordersSUFFIX"

    - **Subscription:** Select the subscription you are using for this hands-on lab

    - **Resource group:** Select **Use existing** and choose the **hands-on-lab-SUFFIX** resource group

    - **OS:** Select Windows

    - **Hosting Plan:** Choose Consumption Plan

    - **Location:** Select the location you have been using for resources in this hands-on lab

    - **Storage:** Select **Create new** and enter "bestforyouorders" for the name

    - Select **Create** to provision the new Function App

        ![The information above is entered on the Create Function App blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image181.png "Create blade")

### Task 2: Configure storage queues

In this task, you will add two storage queues to the storage account provisioned when you created your Function App. These queues will be used to store orders and notifications needing to be processed.

1. In the Azure portal, navigate to the new **bestforyouorders storage account** that was created when you provisioned your Function App, by selecting **Resource groups** from the left-hand menu, selecting your **hands-on-lab-SUFFIX** resource group from the list, and then selecting the **bestforyouorders storage account**

    ![Resource groups is highlighted in the navigation pane of the Azure portal, and hands-on-labs is selected and highlighted to the right under Resource groups. Overview is selected to the right, and the bestforyouorders storage account row is highlighted on the far right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image182.png "Azure Portal")

2. Select **Queues** from the **Services** area of the **Overview** blade

    ![Queues is highlighted in the Services area of the Overview blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image183.png "Overview blade Services area")

3. On the **Queue service** blade, select **+Queue** to add a new queue

    ![+ Queue is highlighted on the Queue service blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image184.png "Queue service blade")

4. In the **Add** queue dialog, enter **orderqueue** for the **Queue name**, and select **OK**

    ![Orderqueue is entered in the Queue name box in the Add queue dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image185.png "Add queue dialog box")

5. Select **+Queue** again, and this time enter "notificationqueue" for the **Queue name**

    ![Notificationqueue is entered in the Queue name box in the Add queue dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image186.png "Add queue dialog box")

### Task 3: Create Cosmos DB trigger function

In this task, you will create a function that will be triggered whenever a document is inserted into the orders collection in your Azure Cosmos DB. This function sends all new orders to a queue for processing and shipping. This function will use a Cosmos DB trigger and an output binding to an Azure Storage Queue.

1. When the Function App has deployed, navigate to the new Function App in the Azure portal, by selecting the **notifications icon**, then select **Go to resource** for the Function App notification

    ![The Go to resource button is highlighted under a Deployment succeeded message in a Notifications window.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image187.png "Notifications window")

2. From the left-hand menu on your **Function Apps** blade, select **Functions**, then select **+New function**

    ![Functions is selected and highlighted in the left-hand menu on the Function Apps blade, and + New function is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image188.png "Function Apps blade")

3. In the trigger search box, enter "cosmos," and select the **Cosmos DB trigger**

    ![In the trigger search box, cosmos is selected, and the Cosmos DB trigger is selected below it.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image189.png "Choose a template page")

4. In the **Cosmos DB trigger** dialog, enter the following:

    - **Language**: Select **JavaScript**

    - **Name**: Enter "OrdersCosmosTrigger"

    - **Azure Cosmos DB account connection**: Select **new**, then select the **best-for-you-db DocumentDB** Account

    - **Collection name:** Enter orders (use all lowercase, as case matters)

    - **Create lease collection if it does not exist:** Leave this checked

    - **Database name:** Enter "best-for-you-organics"

    - **Collection name for leases:** Leave set to leases

    - Select **Create**

        ![The information above is entered in the Cosmos DB blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image190.png "Cosmos DB blade")

5. After the function is created, select **Integrate** under the new function

    ![Integrate is selected and highlighted under the new function.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image191.png "New function menu")

6. Change the Document collection parameter name to "newOrders" for the **Azure Cosmos DB trigger** and select **Save**

    ![The newOrders value is entered in the Document collection parameter name box for the Azure Cosmos DB trigger, and Save is selected at the bottom.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image192.png "Azure Cosmos DB trigger section")

7. Next, select **+New Output**, select **Azure Queue Storage**, and select **Select**

    ![+ New Output is highlighted under Outputs, Azure Queue Storage is selected below it, and Select is selected at the bottom.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image193.png "Select Azure Queue Storage")

8. For the **Azure Queue Storage output**, enter the following:

    - **Message parameter name**: outputQueue

    - **Queue name:** orderqueue (all lowercase, as casing matters)

    - **Storage account collection:** Select **AzureWebJobsStorage** from the list (this is the bestforyouorders storage account you created when you provisioned your Function App)

    - Select **Save**

    ![The information above is entered in the Azure Queue Storage output dialog box, and Save is selected at the bottom.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image194.png "Azure Queue Storage output page")

9. Now, select the **OrdersCosmosTrigger** function in the left-hand menu

    ![The OrdersCosmosTrigger function is selected in the left-hand menu.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image195.png "Left menu")

10. To get the code for the OrdersCosmosTrigger function, go into the project is VS Code, expand the AzureFunctions folder, select **OrdersCosmosTrigger.js**, and copy the code, as highlighted in the screen shot below

    ![Under the AzureFunctions folder in Visual Studio Code, OrdersCosmosTrigger.js is selected and highlighted in Explorer. On the right, the code for the OrdersCosmosTrigger function is highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image196.png "Visual Studio Code")

11. Paste the code into the **index.js** block, overwriting all the existing code, and select **Save**. Your index.js file should now look like the following:

    ![This is a screenshot of the index.js block.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image197.png "Index.js block")

12. Next, select **Logs** below the code block, so you can observe the Function being called during the next steps

    ![Logs is highlighted below the code block.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image198.png "Select Logs")

13. To trigger the function, return to the starter application in your browser window, and select **Sign In**

    ![Two Person Plan, High-Pro Plan, and Four Person Plan boxes are visible in this screenshot of the starter application, and Sign In is highlighted at the top.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image199.png "Sign in to the starter application")

14. On the Login screen, enter the following credentials, and select **Login**

    - **Email address:** <demouser@bfyo.com>

    - **Password:** Password.1!!

    ![The credentials above are entered in the Login page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image200.png "Login page")

15. After logging in, you will be returned to the home page. Select **Select this plan** for any of the plans

    ![Two Person Plan, High-Pro Plan, and Four Person Plan boxes are visible in this screenshot of the home page, and all three boxes' Select this plan buttons are highlighted.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image201.png "Select a plan")

16. On the **Place Order** screen, select **Place Order**. This will create a new order, which will fire the Azure Cosmos DB trigger in your function, and then send the order on to the ordersqueue for processing

    ![The Place Order button is highlighted at the bottom of the Place Order page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image202.png "Place your order page")

17. Next, you will update an order in the Azure portal, to set the processed value to true. This will be a change that should not be sent into the orderqueue for processing

18. Navigate to your Cosmos DB account in the Azure portal, select **Data Explorer**, expand the **orders** collection, then select **Documents**

    ![Data Explorer is selected and highlighted on the left side of the Cosmos DB account in the Azure portal, and Documents is selected and highlighted in the expanded orders collection.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image203.png "Azure Cosmos DB account blade")

19. Select any order document, and change the processed value to "true," then select **Update**

    ![Update is highlighted at the top of a document, and true is highlighted next to the processed value.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image204.png "Change the processed value to true")

20. Return to the logs pane of your function and observe that the orders have been processed though the Function, and that the new order was sent to the orderqueue, while the updated order was not

    ![The new order and the updated order are highlighted in the logs pane of your function.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image205.png "Logs pane")

21. Finally, verify items are being written to the order queue, by going to the queue in the Azure Storage account, and observing that items have been added to the queue

    ![The Refresh button is highlighted in the Azure Storage account, and Message Text appears in the order queue below.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image206.png "Messages blade")

### Task 4: Create Queue function

In this task, you will create a second function which will be triggered by the output of the OrdersCosmosTrigger function. This will simulate the order processing and will add items to the notificationqueue if the order processing is complete and sendNotifications is true for the order.

This will use an Azure Storage Queue trigger, and an input dataset from Cosmos DB, pulling in customers. Output dataset will be Azure Cosmos DB orders table, and an update to set processed = true, and the processedDate to today.

1. Select **Integrate** under the OrdersCosmosTrigger function, then select **Azure Queue Storage (outputQueue)** under **Outputs**

    ![Azure Queue Storage (outputQueue) is selected and highlighted under Outputs in the OrdersCosmosTrigger function.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image207.png "Outputs section")

2. Under **Actions** for the output, select **Go** next to **Create a new function triggered by this output**

    ![The Go button is next to Create a new function triggered by this output under Actions.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image208.png "Actions section")

3. Select **Queue trigger** from the list

    ![This is a screenshot of the Queue trigger box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image209.png "Queue trigger box")

4. On the **Queue trigger New Function** dialog, enter the following:

    - **Language:** Select **JavaScript**

    - **Name:** Enter ProcessOrders

    - **Queue name:** orderqueue

    - **Storage account connection:** Select **AzureWebJobsStorage**

    - Select **Create**

        ![The information above is entered in the Queue trigger New Function dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image210.png "Queue trigger New Function dialog box")

5. When the function has been created, select **Integrate** under the **ProcessOrders** function, change the Message parameter name to "orderToProcess" for the **Azure Queue storage trigger**, and select **Save**

    ![Integrate is selected and highlighted under the ProcessOrders function on the left, and orderToProcess is highlighted in the Message parameter name box on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image211.png "Azure Queue Storage trigger section")

6. Now, select **+New Input**, select **Azure Cosmos DB**, and select **Select**

    ![+ New Input is highlighted under Inputs, Azure Cosmos DB is selected below it, and Select is selected at the bottom.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image212.png "Inputs section")

7. On the **Azure Cosmos DB** input screen, enter the following:

    - **Document parameter name:** Enter "users"

    - **Database name:** Enter "best-for-you-organics"

    - **Collection name:** Enter "users" (all lowercase, as case matters)

    - **Azure Cosmos DB account connection:** Select **best-for-you_DOCUMENTDB**

    - Select **Save**

    ![The information above is entered in the Azure Cosmos DB input page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image213.png "Azure Cosmos DB input page")

8. Next, select **+New Output**, select **Azure Queue Storage**, and select **Select***

    ![+ New Output is highlighted under Outputs, Azure Queue Storage is selected below it, and Select is selected at the bottom.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image214.png "Outputs section")

9. For the **Azure Queue Storage output**, enter the following:

    - **Message parameter name**: "outputQueue"

    - **Queue name:** "notificationqueue" (all lowercase, as casing matters)

    - **Storage account collection:** Select **AzureWebJobsStorage** from the list

    - Select **Save**

    ![The information above is entered in the Azure Queue Storage output dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image215.png "Azure Queue Storage output dialog box")

10. Now, select the **ProcessOrders** function in the left-hand menu

    ![The ProcessOrders function is selected and highlighted in the left-hand menu.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image216.png "Left menu")

11. To get the code for the **ProcessOrders** function, go into the project is VS Code, expand the **AzureFunctions** folder, select **ProcessOrders.js**, and copy the code, as highlighted in the screen shot below

    ![ProcessOrders.js is highlighted on the left side of Visual Studio Code, and the code for the ProcessOrders function is highlighted on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image217.png "Visual Studio Code")

12. Paste the code into the **index.js** block, overwriting all the existing code, and select **Save**. Your index.js file should now look like the following:

    ![The code for the ProcessOrders function is pasted in the index.js block.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image218.png "Index.js block")

13. Next, select **Logs** below the code block, so you can observe the Function being called during the next steps

    ![Logs is highlighted below the code block.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image198.png "Select Logs")

14. To trigger the function, return to the starter application in your browser window, select **Sign In**, and on the **Sign In** screen, select **Register**

    ![In this screenshot of the starter application, Sign In is highlighted at the top, and the Register button is highlighted below.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image219.png "Sign in to the starter application")

15. Complete the registration form, being sure to include a cell phone number in the Phone field that you can receive text messages on. (If you opt not to enter a phone number, you can still complete the next Exercise, but will not receive the text notifications that your order has been processed.)

    - You only need to enter data into the **First name**, **Last name**, **email address**, and **phone** fields. All other fields have been pre-populated to save time

    - The password has been set to Password.1!!. If you choose to enter a different password, note that when you log into the account

16. After registering, select **Sign In** from the Home page, enter your email address and password (Password.1!!) on the login screen, and select **Login**

17. Select the **Select this plan** button for any plan on the home page, and on the Order screen, select **Place Order**

    ![The Place Order button is highlighted on the Order page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image202.png "Order page")

18. Return to your **ProcessOrders Function** page in the Azure portal and observe the logs

    ![A notification is highlighted in the logs on the ProcessOrders Function page in the Azure portal.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image220.png "ProcessOrders Function page")

19. The order you placed has been sent to the notificationqueue and is pending the notification being sent to your cell phone via SMS text message

## Exercise 7: Create Logic App for sending SMS notifications

Duration: 30 minutes

In this exercise, you will create Logic App which will trigger when an item is added to the notificationqueue Azure Storage Queue. The Logic App will use a Twilio connection to send an SMS message to the phone number included in the notificationqueue message.

### Task 1: Create Free Twilio account

In this task, you will use a free Twilio account to send SMS notifications to customers, informing them that their order has been processed, and is on its way.

1. If you do not have a Twilio account, sign up for one for free at by going to <https://www.twilio.com/try-twilio>

2. On the **Sign up for free** page:

    - Enter your personal info, email address, and a 14+ character password

    - Select SMS under **Which product do you plan to use first?**

    - Select **Order Notifications** under **What are you building?**

    - Select **JavaScript** under **Choose your language**

    - Select **Not a Production App** under **Potential monthly interactions**

    - Check the box next to **I'm not a robot**

    - Select **Get Started**

    ![The information above is entered on the Sign up for free page.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image221.png "Sign up for free page")

3. Enter your **cell phone number** on the We need to verify you're a human screen, check the box if you do not wish to be contacted at the number you enter, and select **Verify** via SMS

    ![An obscured cell phone number is entered next to the Verify via SMS button on the We need to verify you're a human screen.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image222.png "Verify your human screen")

4. Enter the verification code received via text into the box and select **Submit**

    ![A verification code is entered on the We need to verify you're a human screen.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image223.png "We need to verify you???re a human screen")

5. From your account dashboard, select the **All Products & Services icon**

    ![The All Products & Services icon is highlighted on your account dashboard. ](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image224.png "Accounts Dashboard")

6. Select **\#Phone Numbers** under **Super Network**

    ![\#Phone Numbers is highlighted under Super Network.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image225.png "Super Network section")

7. Select **Get Started**

    ![Get Started is highlighted on the Phone Numbers Dashboard screen.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image226.png "Phone Numbers Dashboard screen")

8. Select **Get your first Twilio phone number**

    ![Get your first Twilio phone number is highlighted on the Get Started with Phone Numbers screen.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image227.png "Get Started with Phone Numbers screen")

9. Select **Choose this Number** (or search for a different number if you want something different)

    ![Choose this Number is highlighted on the Your first Twilio Phone Number screen.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image228.png "Your first Twilio Phone Number screen")

10. Select **Done** on the Congratulations dialog

11. Select **Home**

    ![Done is highlighted on the Congratulations! screen.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image229.png "Congratulations screen")on your **Account Dashboard**, and leave this page up, as you will be referencing the **Account SID** and **Auth Token** in the next task to configure the Twilio Connector.![The Home icon is highlighted on your Account Dashboard.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image230.png "Account dashboard")

### Task 2: Create Logic App

In this task, you will create a new Logic App, which will use the Twilio connector to send SMS notifications to users, informing them that their weekly order has processed and shipped.

1. In the Azure portal, select **+Create a resource**, select **Web + Mobile**, and select **Logic App**

    ![+ Create a resource is highlighted on the left side of the Azure portal, Web + Mobile is selected in the middle, and Logic App is selected on the right.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image231.png "Azure Portal")

2. In the **Create logic app** blade, enter the following:

    - **Name:** Enter "OrderNotifications"

    - **Subscription:** Select the subscription you are using for this hands-on lab

    - **Resource group:** Select **Use existing** and choose the **hands-on-lab-SUFFIX** resource group

    - **Location:** Select the location you have been using for resources in this hands-on lab

    - Select **Create** to provision the new Logic App

        ![The information above is entered on the Create logic app blade.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image232.png "Logic App blade")

3. Navigate to your newly created Logic App in the Azure portal

4. Select the **Logic App Designer** under **Development Tools** on the left-hand menu

    ![Logic App Designer is selected under Development Tools in the left-hand menu of your newly created Logic App.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image233.png "Development Tools section")

5. In the Logic App Designer, select **Blank Logic App** under **Templates**

    ![Blank Logic App is highlighted under Templates in Logic App Designer Templates section.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image234.png "Logic App Designer, Templates section")

6. Select **Azure Queues** under **Connectors**

    ![Azure Queues is highlighted under Connectors.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image235.png "Connectors section")

7. Select **Azure Queues** -- When there are messages in a queue

    ![Azure Queues -- When there are messages in a queue is highlighted under Triggers (2).](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image236.png "Triggers section")

8. On the When there are messages in a queue dialog, enter **bestforyouorders** for the, **Connection Name** select the bestforyouorders **Storage account** from the list and select **Create**

    ![In the When there are messages in a queue dialog box, Bestforyouorders is in the Connection Name box, the bestforyouorders row is highlighted at the top of the list below Storage account, and the Create button is highlighted at the bottom.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image237.png "When there are messages in a queue dialog box")

9. In the next When there are messages in a queue dialog, select **notificationqueue** from the **Queue Name** list, and set the interval to **1** **minute**, then select **+New step**, and **Add an action**

    ![The information above is entered in the When there are messages in a queue dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image238.png "When there are messages in a queue dialog box")

10. In the **Choose an action box**, enter "Parse," and select **Data Operations** **-- Parse JSON** from the list

    ![In the When there are messages in a queue dialog box, Parse is in the Choose an action box, and Data Operations -- Parse JSON is highlighted below in the list.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image239.png "When there are messages in a queue dialog box")

11. Select the **Content** box, select **Add dynamic content +**, then select **Message Text** from the input parameters list that appears

    ![In the Parse JSON window, Message Text is in the Content box, Add dynamic content is highlighted, and Message Text is highlighted below in the input parameters list.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image240.png "Parse JSON window")

12. Next, select **Use sample payload to generate schema** below the **Schema** box

    ![In the Parse JSON window, Use sample payload to generate schema is highlighted below the Schema box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image241.png "Parse JSON window")

13. In the dialog that appears, paste the following JSON into the sample JSON payload dialog that appears, then select **Done**

    ```json
    {"orderId":"5a6748c5d0d3199cfa076ed3","userId":"demouser@bfyo.com","notificationPhone":"3175551212","firstName":"Demo"}
    ```

    ![The JSON above is pasted in the sample JSON payload dialog box, and Done is selected below.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image242.png "Paste the JSON in the dialog box")

14. You will now see the Schema for messages coming from the notification queue in the Schema box. Select **+New** **step** and select **Add an action**

    ![The Add an action button under + New step is highlighted in the Schema box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image243.png "Parse JSON window")

15. In the **Choose an action box**, enter "Twilio," and select **Twilio -- Send Text Message (SMS)** under Actions

    ![Twilio is highlighted in the Choose an action box, and Twilio -- Send Text Message (SMS) is highlighted under Actions.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image244.png "Choose an action box")

16. In the **Twilio -- Send Text Message (SMS)** dialog, enter the following (You will need the details from Project Info block on the dashboard of your Twilio account for this step):

    - **Connection Name:** Twilio

    - **Twilio Account Id:** Enter your Twilio account SID

    - **Twilio Access Token:** Enter your Twilio auth token

    - Select **Create**

        ![The information above is entered in the Twilio -- Send Text Message (SMS) dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image245.png "Twilio ??? Send Text Message (SMS) dialog box")

17. On the next **Send Text Message (SMS)** dialog, enter the following:

    - **From Phone Number:** Select your Twilio phone number from the drop down

    - **To Phone Number:** Select **notificationPhone** from the **Parse JSON** parameters

    ![The information above is entered in the next Send Text Message (SMS) dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image246.png "Send Text Message (SMS) dialog box")

    - **Text:** Enter a message, such as "Hello \[firstName\], your Best for You Organics weekly order has shipped!" For \[firstName\], select the **firstName** parameter from the **Parse JSON** items

    ![The information above is entered in the next Send Text Message (SMS) dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image247.png "Send Text Message (SMS) dialog box")

18. Select **+New step** and **Add an action**

    ![The Add an action button is highlighted under + New step.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image248.png "Add an action button")

19. In the **Choose an action** dialog, enter "queue" in to the search box, and select **Azure Queues -- Delete** **message**

    ![Queue is highlighted in the Choose an action search box, and Azure Queues -- Delete message is highlighted below.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image249.png "Choose an action dialog box")

20. Select **notificationqueue** for the Queue Name

21. For Message ID, select the **Message ID** parameter from the **When there are messages in the queue** parameter list

    ![The information above is entered in the Delete message dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image250.png "Delete message dialog box")

22. For Pop Receipt, select the **Pop Receipt** parameter from the **When there are messages in a queue** parameter list

    ![The information above is entered in the next Delete message dialog box.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image251.png "Delete message dialog box")

23. Select **Save** on the **Logic Apps Designer** toolbar

    ![Save is highlighted on the Logic Apps Designer blade toolbar.](images/Hands-onlabstep-bystep-OSSPaaSandDevOpsimages/media/image252.png "Logic Apps Designer blade")

24. The Logic App will begin running immediately, so if you entered your cell phone number when you registered your account in the Best for You Organics starter app, and placed an order, you should receive a text message on your phone within a minute or two of selecting Save

## After the hands-on lab

Duration: 10 minutes

In this exercise, you will de-provision all Azure resources that were created in support of this hands-on lab.

### Task 1: Delete Azure resource groups

1. In the Azure portal, select **Resource groups** from the left-hand menu, and locate and delete the following resource groups.

    - hands-on-lab-SUFFIX
    - jenkins-SUFFIX

### Task 2: Delete WebHooks and Service Integrations

1. In your GitHub account

    - Delete the Jenkins service integration
    - Delete the Personal Access Token you created for integration with VS Code

You should follow all steps provided *after* attending the Hands-on lab.
