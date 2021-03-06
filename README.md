<p align="center">
  <img src="./assets/logo.png">
</p>
<h4 align="center">
	A <a href="https://en.wikipedia.org/wiki/Headless_content_management_system">headless CMS</a> built in a <a href="https://deno.land">Deno</a> runtime environment.
</h4>

<p align="center">
  <a href="https://github.com/oslabs-beta/hollow/blob/main/LICENSE"><img alt="license" src="https://img.shields.io/github/license/oslabs-beta/hollow?color=%23e87e4d&style=flat-square" ></a>
  <a href="https://github.com/oslabs-beta/hollow/pulls?q=is%3Apr+is%3Aclosed"><img alt="last commit" src="https://img.shields.io/github/last-commit/oslabs-beta/hollow?color=%23e87e4d&style=flat-square" ></a>
  <a href="https://github.com/oslabs-beta/hollow/issues"><img alt="open issues" src="https://img.shields.io/github/issues-raw/oslabs-beta/hollow?color=%23bd5555&style=flat-square" ></a>
  <a href="https://github.com/oslabs-beta/hollow/graphs/contributors"><img alt="contributors" src="https://img.shields.io/github/contributors/oslabs-beta/hollow?color=%23e87e4d&style=flat-square" ></a>
</p>

<p align="center">
   <img alt="stars" src="https://img.shields.io/github/stars/oslabs-beta/hollow?style=social" >
</p>

## Overview
[Hollow](http://hollowcms.land) is a first-of-its-kind headless CMS generating a backend exclusively in Deno. Hollow offers straightforward deployment through AWS, making it easy to manage your database, add content, and get an API up and running in minutes.

## Table of Contents
* [Installation](#Installation)
* [Guides](#Guides)
  * [Adding a New Collection](#Adding-a-New-Collection)
  * [Deleting a Collection](#Deleting-a-Collection)
  * [Adding a Field to Collection](#Adding-a-Field-to-Collection)
  * [Editing a Collection's Field](#Editing-a-Collections-Field)
  * [Deleting a Collection's Field](#Deleting-a-Collections-Field)
  * [Adding an Entry to a Collection](#Adding-an-Entry-to-a-Collection)
  * [Editing an Entry in a Collection](#Editing-an-Entry-in-a-Collection)
  * [Deleting an Entry from a Collection](#Deleting-an-Entry-from-a-Collection)
* [API Endpoints](#API-Endpoints)
* [How To Contribute](#How-To-Contribute)
* [Contributors](#Contributors)

## Installation
Hollow is currently only configured for AWS. Create an AWS account [here](https://aws.amazon.com/console/) if you haven't already.

**WARNING:** You may incur charges by using AWS services. Please proceed at your own risk.
### Create an Elastic Beanstalk Instance
1. **Create a zipped repository.**
	- Clone this repo to your local machine by running `git clone https://github.com/oslabs-beta/hollow.git`.
	- In order to deploy Hollow, you'll need to zip the repo into an archive file. Navigate to your Hollow directory in Terminal and run the following command: `git archive -v -o hollow.zip --format=zip HEAD`.
2. **Create a new Elastic Beanstalk instance.**
	- Navigate to **Elastic Beanstalk** via the **Services** menu in AWS and click **Create Application**.
	- Give your application a name (e.g. `Hollow`).
	- Under **Platform**, select Docker. Leave the platform branch and version as is (Docker running on 64bit Amazon Linux 2, v3.2.4).
	- Under **Application code**, select **Upload your code**. In the **Source code origin** panel, click **Choose file** and upload the `hollow.zip` file generated in the previous step.
	- Click **Create application**.
### Create an RDS Database
1. **Create a new database.**
	- Navigate to **RDS** via the **Services** menu and click **Create database**.
	- Leave the database creation method as **Standard create**.
	- Under **Engine options**, select PostgreSQL, leaving the version as is.  Select **Free tier** in the **Templates** panel if you wish to avoid charges.
	- In the **Settings** panel, provide your database instance with a meaningful name, like `hollowdb-instance-1`. Create a master username and password. (*Write these down!* These will be your `RDS_USERNAME` and `RDS_PASSWORD` environment variables.)
	- In the **Storage** panel, edit your database instance size, storage type, and allocated storage as needed.
	- In the **Connectivity** panel, select **Create new** under **VPC security group**. Give the group the name `hollow-db-sg` and select the availability zone closest to you.
	- In the **Additional Configuration** panel, create an Initial Database Name. (*Write this down!* This will be your `RDS_DB_NAME` environment variable.)
	- Click **Create database**.
	- You'll need the endpoint for your RDS instance. Find this under **DB Instances**. This will be your `RDS_HOSTNAME` environment variable later.
2. **Modify your security group settings.**
	- Navigate to **EC2** via the **Services** menu.
	- Click **Security groups** under **Resources** and find your database security group (the security group name should be `hollow-db`). Click the security group ID.
	- In the **Inbound rules** panel, click **Edit inbound rules** and **Add rule**.
	- Select PostgreSQL for **Type** and Custom for **Source**. Under **Source**, select the security group(s) associated with your Elastic Beanstalk EC2 environment.
	- Click **Save rules**.
### Connect to Your Database
1. **Set your database environment variables.**
	- Navigate back to **Elastic Beanstalk** via the **Services** menu.
	- In the sidebar under your Hollow production environment, navigate to **Configuration** > **Software**.
	- You'll need to give Hollow access to your RDS credentials by passing them as environment variables. Add the following variables under **Environment properties**:
		-   `RDS_HOSTNAME`: [your RDS endpoint]
		-   `RDS_DB_NAME`: [your RDS initial database name]
		-   `RDS_USERNAME`: [your username]
		-   `RDS_PASSWORD`: [your password]
		-   `RDS_PORT`: 5432
	- Click **Apply**.
2. **Go forth and conquer!**
	- Navigate to **EC2** via the **Services** menu.
	- Under your Hollow production environment, click **Go to environment**.
	- Your new Hollow application should be ready to go with your newly created database.

## Guides

### Adding a New Collection
1. To add a new collection, navigate to the 'Tools' section in the main sidebar and click on 'Content-Builder'. This will open the Content-Builder page.
2. In the Content-Builder sidebar that opens up, click on 'Add New Collection'.
3. Fill out the input's and click on 'Add Collection' to add the collection to your database.

![ADD COLLECTION GIF](https://github.com/oslabs-beta/hollow/blob/main/assets/add_collection.gif)

### Deleting a Collection
1. To delete a collection, navigate to the 'Collections' section in the main sidebar and click on the collection you want to delete. This will open up the collection's page.
2. On the upper left hand side of the collection's page click the red trash can. This will open a confirmation popup.
3. Confirm that you want to delete the collection. This will drop the selected collection from your database.

![DELETE COLLECTION GIF](https://github.com/oslabs-beta/hollow/blob/main/assets/delete_collection.gif)

### Adding a Field to Collection
1. To add a field to a collection, navigate to the 'Tools' section in the main sidebar and click on 'Content-Builder'. This will open the Content-Builder page. 
2. In the Content-Builder sidebar that opens up, click on the collection you want to add the field in. This will open up the collection's field's page.
3. In the upper right hand side of the Content-Builder page, click on 'Add New Field'. This will open the Add Field popup.
4. Fill out the input's and click on 'Add Field' to update the selected collection's field in your database.

![ADD FIELD GIF](https://github.com/oslabs-beta/hollow/blob/main/assets/add_field.gif)

### Editing a Collections Field

**WARNING:** Updating a field's data type will overwrite any data currently stored under that field. Updating to a boolean defaults to true, while updating to an integer defaults to 1. 

 **NOTE:** You cannot edit the id field. It is required on every collection and is serialized automatically.

1. To edit a collection's field, navigate to the 'Tools' section in the main sidebar and click on 'Content-Builder'. This will open the Content-Builder page.
2. In the Content-Builder sidebar that opens up, click on the collection you want to edit the field in. This will open up the collection's field's page.
3. In the table that appears, click on the row with the field you want to edit. This will open up that field's page.
4. Edit the input fields.
5. In the upper right hand corner, click the 'Save' button to update the field in your databasae.

![EDIT FIELD GIF](https://github.com/oslabs-beta/hollow/blob/main/assets/edit_field.gif)

### Deleting a Collections Field
1. To delete a collection's field, navigate to the 'Tools' section in the main sidebar and click on 'Content-Builder'. This will open the Content-Builder page. 
2. In the Content-Builder sidebar that opens up, click on the collection containing the field you want to delete. This will open up the collection's field's page.
3. In the upper left hand side of the field's page, click the red trash can. This will open a confirmation popup.
4. Confirm that you want to delete the field. This will delete the field, along with any data that may be saved under it, from the selected collection, in your database.

![DELETE FIELD GIF](https://github.com/oslabs-beta/hollow/blob/main/assets/delete_field.gif)

### Adding an Entry to a Collection
1. To add an entry to a collection, navigate to the 'Collection's' section in the main sidebar and click on the collection you want to add an entry to. This will open up the collection's page.
2. In the upper right hand side of the collection's page, click on 'Add New <collection>'. This will open up the Add Entry page.
3. Fill out the input fields.
4. In the upper right hand side of the Add Entry page, click on 'Add Entry' to add the entry to selected colleciton in your database.

![ADD ENTRY GIF](https://github.com/oslabs-beta/hollow/blob/main/assets/add_entry.gif)

### Editing an Entry in a Collection

**NOTE:** An entry's id cannot be edited.

1. To edit an entry in a collection, navigate to the 'Collection's' section in the main sidebar and click on the collection containing the entry you want to edit. This will open up the Edit Entry page.
2. Edit the input's to reflect the changes you want to make.
3. In the upper right hand side of the Edit Entry page, click on 'Save' to update the entry in the selected collection, in your database.

![EDIT ENTRY GIF](https://github.com/oslabs-beta/hollow/blob/main/assets/edit_entry.gif)

### Deleting an Entry from a Collection
1. To delete an entry in a colleciton, navigate to the 'Collections' section in the main sidebar and click on the collection containing the entry you want to delete. This will open up the Edit Entry page.
2. In the upper right hand side of the Edit Entry page, click on the red trash can. This will open a confirmation popup.
3. Confirm you want to delete the entry. This will delete the entry from the selected collection, in your database.

![DELETE ENTRY GIF](https://github.com/oslabs-beta/hollow/blob/main/assets/delete_entry.gif)

## API Endpoints
*  Get all items in a collection: ```GET``` ```/api/<collection-name>```
*  Get one item in a collection: ```GET``` ```/api/<collection-name>/<item-id>```
*  Add item to a collection: ```POST``` ```/api/<collection-name>```
*  Update item in a collection: ```PUT``` ```/api/<collection-name>/<item-id>```
*  Delete item in a collection: ```DELETE``` ```/api/<collection-name>/<item-id>```

## How To Contribute
If you come accross an issue you want to solve or have a feature you'd like to add, please fork this repo. Commit changes to a feature branch then open a pull request. We would love to see this project grow!

**These features are at the top of our list:**
* Building out a tool to handle setting roles & permissions
* Adding support for more data types
* Adding support for more DBMS's
* Adding support for GraphQL
* Adding settings for the Admin Tool

## Maintainers
* [Matt Greenberg](https://github.com/mattagreenberg)
* [Shelby Cotton](https://github.com/shelbycotton)
* [Keith Lisiak](https://github.com/keithcoach)
* [Garrett Weaver](https://github.com/G-Weaver)
