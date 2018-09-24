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

  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete Azure resource groups](#task-1-delete-azure-resource-groups)
    - [Task 2: Delete WebHooks and Service Integrations](#task-2-delete-webhooks-and-service-integrations)

# OSS PaaS and DevOps hands-on lab step-by-step

## Abstract and learning objectives

In this hands-on lab, you will implement a solution for integrating and deploying complex Open Source Software (OSS) workloads into Azure PaaS. You will migrate an existing MERN (MongoDB, Express.js, React.js, Node.js) stack application from a hosted environment into Azure Web App for Containers, migrate a MongoDB instance into Cosmos DB, enhance application functionality using serverless technologies, and fully embrace modern DevOps tools.

At the end of this hands-on lab, you will be better able to migrate and deploy OSS applications into Azure PaaS using modern DevOps methodologies and Docker containers.

## Overview

Best For You Organics Company is one of the leading health food suppliers in North America, serving customers in Canada, Mexico, and the United States. They have a MERN stack web application which they host on-premises and are looking to migrate their OSS application into Azure. They will be creating a custom Docker image for their application and using Azure DevOps continuous integration/continuous delivery (CI/CD) pipeline to deploy the application into a Web App for Containers instance. In addition, they will be setting up Azure Cosmos DB, using the MongoDB APIs, so they do not have to make application code changes to leverage the power of Cosmos DB.

In this hands-on lab, you will assist with completing the OSS application and database migrations into Azure. You will create a custom Docker image, provision an Azure Container Registry, push the image to the registry, and configure the CI/CD pipeline to deploy the application to a Web App for Containers instance.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![This diagram consists of icons that are connected by arrows. On the left, the Developer icon (VS Code) points in a linear fashion to the GitHub Repo and Azure DevOps icons. The previous two icons are enclosed in a box labeled CI/CD Pipeline. Azure DevOps points to Web App for Containers on the right. Various arrows point from Web App for Container to: Azure Container Registry (a double-sided arrow); Logic Apps (a linear arrow that also points from Logic Apps to Customers); Customers (a linear arrow); and Azure Cosmos DB (a double-sided arrow that also points from Azure Cosmos DB to Azure Functions with another double-sided arrow).](media/solution-architecture-diagram.png "Solution architecture diagram")

The solution begins with developers using Visual Studio Code (VS Code) as their code editor, so they can leverage its rich integration with GitHub, Docker, and Azure. From a high level, developers will package the entire OSS application inside a custom Docker container using the Docker extension in VS Code. The image will be pushed to an Azure Container Registry as part of a continuous integration/continuous delivery (CI/CD) pipeline using GitHub and Azure DevOps. This Docker image will then be deployed to a Web App for Containers instance, as part of their continuous delivery process.

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


## After the hands-on lab

Duration: 10 minutes

In this exercise, you will de-provision all Azure resources that were created in support of this hands-on lab.

### Task 1: Delete Azure resource groups

1. In the Azure portal, select **Resource groups** from the left-hand menu, and locate and delete the following resource groups.

    - hands-on-lab-SUFFIX
You should follow all steps provided *after* attending the Hands-on lab.
