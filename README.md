# RAMP-Docs
Documentation for the RAMP Framework

https://trifinbholcombe.github.io/RAMP-Docs/


## Installation Guide
Create and launch a new Heroku App from the RAMP API Github repo

1. Navigate to **https://github.com/trifinlabs/ramp-api**
1. Click the **Heroku** button to deploy the RAMP API modules.
1. On this screen, you will need to name the application, and set the app owner. NOTE: You will need a verified account in order to fully setup the API.
1. Select **Deploy App**.
1. After deploying the application, you can verify the application deployed by clicking **‘View’** and it will show you a list of RAMP objects.

## Install Managed Package in Salesforce
To get this started, navigate to your Salesforce Org.

1. Copy and paste the RAMP package URL: **/packaging/installPackage.apexp?p0=04t41000002O4I9**
1. Enter the password
1. Click **install only for admins**, then click install.

This step will provision all of our RAMP objects and will only take a few seconds. You will receive an email confirmation once the package is installed.

## Heroku Connect Environment Setup
After the package is installed, we'll go back to Heroku to connect the Salesforce org to Heroku.

1. Click on the Heroku Connect addon to begin the configuration.
1. Next, click set-up connection. Our database will be Postgres.
1. You will be prompted to login to Salesforce (SF).
1. When you see the message provisioning the database you will have to choose your SF environment. Once you login to SF, click allow to complete the connection to Salesforce. You can now pull up all of the metadata from the SF org to setup the object/table mappings.
1. Click create mapping. This will load all the metadata and objects from Salesforce. For demo purposes we will choose a couple RAMP package objects.
1. First, find and select the Organization object.
1. Then, Select all the fields to sync, set the poll frequency, etc. Click accelerate polling for enhanced performance. We use an external ID to update as our unique identifier.
1. Now we create our field mappings. If you want to map a field in the database simply click on the field and then click save. Now we've created our Salesforce schema and our first table in Postgres which will be the organization.

## Local Development Setup
1. Download the Github repository: **https://github.com/trifinlabs/ramp-api**
1. Create a new repository either on Github or any other managed repository of your choice (bitbucket, SVN, etc) in order to manage your own RAMP API development.
1. Navigate to your local project and create a new file called .env.
1. Add all of the config variables created in the Heroku app creation steps to this .env file.
1. Run the following commands from the root folder of the project:
    ```
    npm install
    ```
    ###### Installs the ramp secure API and other necessary node packages.
    ---

    ```
    npm i -g ramp-api-cli
    ```
    ###### Installs the CLI package needed to add in the models.
    ---

    ```
    ramp-api-cli-win.cmd --allNewModels
    ```
    ###### Sets up the models folder with the ramp objects. This command can be run for any new models that need to be created or added  via salesforce.

1. Once the commands have been run and the .env file setup, the local API is ready to be started via the following command:
    ```
    npm run start-win
    ```
    ###### If you are on MacOS replace all instances of ‘win’ with ‘macos’. If you are on Linux remove all instances of ‘win’.
1. Verify that your local server is running by opening a browser and navigating to **http://localhost:5000/explorer**. You should see all of the current models in the model folder in the API explorer.
    ###### NOTE: As changes are made to the code and need to be redeployed to heroku, you will need to setup a deployment pattern on your repository created in Step #2 above (either via Heroku command line, bitbucket pipelines, codeship, etc).

## Local Development Walkthrough
The local environment has a number of configuration files and packages. This section will walk the user through each of these files and packages to gain a better understanding of the RAMP API and how to develop the API further. Below is an image of all of the potential files and folders that can be added to the API to expand functionality.

_ramp api file list photo tbd_

The API is versioned based on a **Node Loopback pattern** and a description of each of these folders can be found on loopback:

1. boot: **https://loopback.io/doc/en/lb2/Defining-boot-scripts.html**
1. components: **https://loopback.io/doc/en/lb2/LoopBack-components.html**
1. mixins: **https://loopback.io/doc/en/lb2/Defining-mixins.html**
1. models: **https://loopback.io/doc/en/lb2/Defining-models.html**

All of the files within these folders will be concatenated with standard RAMP API functionality where possible and will override functionality where not.

**Models** are the workhorse and lifeline connection to both the database and Salesforce. They should be generated using the CLI command from the section above. This command runs directly against the database configured in the .env file and will guarantee the model’s properties will match those properties setup on the database columns in PostGres. Models contain both a .js and a .json file. The .json file contains a name, options, list of properties (columns), field types, ACLs, hidden fields, etc. The .js file for the model will contain any custom REST endpoints, parsers, etc. If you run the CLI command from the setup above each time a new model needs to be added, the models will also be added to the model-config.json file. If you add the models manually, you will need to add the model to the model-config.json file as well.

The RAMP API setup also allows for multiple data source integration. By default the heroku Postgres database datasource is configured for each model created. This datasource is the database provisioned in the initial setup procedure above in Heroku. You can append more data sources to this as well, such as: file systems (AWS, Azure, etc), databases, in memory, and local file systems. The datasources.json file is where these new data sources can be placed. These data sources, like the folders and packages in the local development environment, are concatenated with the default herokuPostgres datasource and will not overwrite functionality.
