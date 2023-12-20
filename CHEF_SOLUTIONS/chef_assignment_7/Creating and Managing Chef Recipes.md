**Part 1: Creating Chef Recipes**

1. Create a New Cookbook
	- create a new cookbook inside your Chef repository directory.
	- code :-
	**chef generate cookbook nginx-cookbook**

2. Create a Recipe for Nginx Installation and Configuration
	- Navigate to the newly created cookbook's `recipes` directory and create a new `.rb` file, `nginx_setup.rb`.
	- example `nginx_setup.rb`:
![[Screenshot from 2023-12-15 17-24-44.png]]

3. Create a Template for Nginx Configuration

	- In the same cookbook directory, create a `templates/default` directory if it doesn't exist. Inside this directory, create an `nginx.conf.erb` file, which will be the template for Nginx's configuration.
![[Screenshot from 2023-12-15 16-57-23.png]]
4. Apply the Recipe to a Node
	- After defining the recipe, upload the cookbook to your Chef Server using `knife` or a similar tool.
	- **knife cookbook upload nginx-cookbook**
![[Screenshot from 2023-12-15 17-03-07.png]]

#### Test your Chef recipe:

1. Create a `.kitchen.yml` File
	- Inside your cookbook's directory, create a `.kitchen.yml` file to define the testing configuration for your recipe. This file specifies the platform, suites, and provisioner settings.
	- Here's an example :
![[Screenshot from 2023-12-15 17-35-50.png]]
2. Converge and Verify
	- Run the `kitchen converge` command to create and provision a virtual machine based on your specified configuration.
	- After the convergence is successful, use the `kitchen verify` command to run the automated tests defined in your cookbook:

**Part 2: Managing Recipes and Environments**

1. Create an Environment File
	- Inside your Chef repository, create a new environment file. Conventionally, these files are stored in the `environments` directory.
	- code :- `mkdir environments`
	- Then, create an environment file, for example, `development.json`.

![[Screenshot from 2023-12-15 18-05-07.png]]
- `name`: Name of the environment (e.g., "development").
- `description`: Description of the environment.
- `cookbook_versions`: Specific versions of cookbooks to be used in this environment.
- `default_attributes`: Default attributes for the `webserver` cookbook specific to this environment. 

2. Assign Nodes or Roles to the Environment
	- After defining the environment, assign nodes or roles to this environment. This can be done either by using the `knife` command.
	- For example: `knife environment from file environments/development.json`

![[Screenshot from 2023-12-15 18-06-29.png]]

3. Upload Cookbook to the Chef Server
	- we have a cookbook named `my_webserver_cookbook` to upload.
	- code :- `knife cookbook upload my_webserver_cookbook`.

4. Upload Environment to the Chef Server
	- we have created an environment file, such as `development.json` , use the following command to upload the environment to the Chef Server.
	- code :-`knife environment from file environments/development.json`

5. Verify Uploads
	- After executing these commands, you can verify that the cookbook and environment have been uploaded successfully by checking them using the `knife` tool.
	- code :-
	- **knife cookbook list**
	- **knife environment list**
- These commands will list all the cookbooks and environments available on the Chef Server. 

6. Apply Environment to Nodes or Roles
	- Once the environment is uploaded, assign it to nodes or roles as mentioned previously using the `knife node` commands.
	- For example, to assign an environment to a node.
	- code :- `knife node environment set chef-node development`

![[Screenshot from 2023-12-15 18-18-18.png]]
