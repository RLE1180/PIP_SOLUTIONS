
**Step 1: Create Three Environments**

**Create Environment JSON Files**:
- For each environment, create a JSON file specifying its settings and attributes. Here are examples for each environment:

1. **Development** : This environment is where you develop and test your cookbooks and recipes. It should be less strict in terms of security and stability.
	- For Example :
// development.json
{
  "name": "Development",
  "description": "Environment for cookbook development and testing",
  "cookbook_versions": { "couchdb": "< 11.0.0"
    // Specify cookbook versions or constraints as needed
  },
  "default_attributes": { "apache2": { "listen_ports": ["80", "443"] }
    // Define less strict security and stability attributes
  }
}

2. **Testing** : The testing environment is used to validate your configurations before deploying to production. It should resemble the production environment as closely as possible.
	- For Example :
// Testing.json
{
  "name": "Testing",
  "description": "Environment to validate configurations before deploying",
  "cookbook_versions": { "couchdb": "< 11.0.0"
    // Specify cookbook versions or constraints as needed
  },
  "default_attributes": { "apache2": { "listen_ports": ["80", "443"] }
    // Define less strict security and stability attributes
  }
}

3. **Production**: The production environment is where your applications run in a stable and secure manner.
	- For Example : 
// production.json
{
  "name": "Production",
  "description": "Stable and secure environment for running applications",
  "cookbook_versions": { "couchdb": "< 11.0.0"
    // Specify cookbook versions or constraints as needed
  },
  "default_attributes": { "apache2": { "listen_ports": ["80", "443"] }
    // Define less strict security and stability attributes
  }
}

**Create Environments** :

- Use the `knife` command to create environments from the JSON files:

**knife environment from file development.json
knife environment from file testing.json
knife environment from file production.json**

![[Screenshot from 2023-12-12 10-56-57.png]]
**Upload Environments to Chef Server** :

- After creating the environments, upload them to the Chef server:   

 **knife environment upload Development
 knife environment upload Testing
 knife environment upload Production**

**Step 2: Configure Environment Attributes**

1. **Edit Environment Attribute Files**:
      - Open the JSON files (e.g., `development.json`, `testing.json`, `production.json`) for each environment in a text editor.
2. **Define Environment-Specific Attributes**:
    - Inside each environment's JSON file, define specific attributes relevant to that environment under the `default_attributes` section.
    - Use a structure similar to the following example to define environment-specific attributes:
    # code
    `// Example for Development environment (development.json)'  
    {
       "name": "Development",   
       "description": "Environment for cookbook development and testing",   "cookbook_versions": { 
       chef_version '>= 16.0'
        },   
        "default_attributes": {
             "database_server_ip": "192.168.122.165",
                  "web_server_config": {
                         "port": 8080,
                                "ssl_enabled": false
                                     }    
                        }
             }`
    
4. **Attribute Examples**:
    - **Database Server IP**:
        - Set the `database_server_ip` attribute to the relevant IP address for the database server in that environment.
    - **Web Server Configuration**:
        - Define `web_server_config` attributes such as `port`, `ssl_enabled`, etc., specific to the web server settings in that environment.
5. **Repeat for Other Environments**:
    - Repeat the process for the 'Testing' and 'Production' environments, defining attributes specific to each environment.
6. **Save Changes**:
    - Save the modified JSON files after defining the environment-specific attributes.

**Step 3: Assign Nodes to Environments**

1. **Node Creation**:
    - Set up test nodes or virtual machines. These could be separate instances or VMs with unique names or IP addresses.
2. **Bootstrap Nodes**:
    - Use the `knife bootstrap` command to bootstrap each node to connect it to the Chef server.
    - Example:
    # syntax :
        `knife bootstrap NODE_IP_OR_HOSTNAME -x USERNAME -P PASSWORD --sudo`
        
    - Replace `NODE_IP_OR_HOSTNAME` with the IP address or hostname of each node, `USERNAME` with the SSH username, and `PASSWORD` with the SSH password.
3. **Assign Nodes to Environments**:
    - Use the `knife node environment set` command to assign each node to its respective environment.
    - For example:
    # syntax :
    **knife node environment set NODE_NAME development
    knife node environment set NODE_NAME testing**
    - Replace `NODE_NAME` with the name of each node you created.
4. **Verification**:
    - Verify the node assignments by checking the node details using the `knife node show` command:
    - For Example :
        `knife node show NODE_NAME`
        
### Example:

Let's assume we have a node: `chef-node` for development.

# code :

`# Bootstrap chef-node and assign it to the Development environment'
**knife bootstrap chef-node -x root -P Sanjay@1180 --sudo** 
**knife node environment set chef-node development**.
![[Screenshot from 2023-12-12 11-52-38.png]]
**knife node show chef-node**
![[Screenshot from 2023-12-12 11-58-38.png]]

**Step 4: Create a Cookbook**

1. **Create a Cookbook** :
	- `chef generate cookbook my_webserver_cookbook`
	- This will create a cookbook named my_webserver_cookbook.
2. **Edit the Recipe** :
	- Navigate to the recipe file within the cookbook (e.g., `recipes/default.rb`) and modify it.
### Example :
![[Screenshot from 2023-12-12 12-39-07 1.png]]
3. **Upload Cookbook to the Chef Server** :
	- upload the cookbook to your Chef server or host it in your Chef environment to make it available for node runs.
# code :
`knife cookbook upload my_webserver_cookbook`
![[Screenshot from 2023-12-12 13-25-57.png]]

**Deploy cookbooks to nodes** : 

After updating the environments with the specific cookbook versions, perform a Chef client run on the nodes in each environment. This will apply the changes based on the updated environments:
#### For Development Environment Nodes:

# syntax :

- `knife ssh 'name:development-node' 'sudo chef-client' -x USERNAME -P PASSWORD`
- Replace `development-node` with the name or identifier of the nodes in the development environment.
- Example :
**knife ssh 'name:development' 'sudo chef-node' -x root -P Sanjay@1180**
#### For Testing Environment Nodes:

# syntax: 

- `knife ssh 'name:testing-node' 'sudo chef-client' -x USERNAME -P PASSWORD`
- Replace `testing-node` with the name or identifier of the nodes in the testing environment.
- Example : 
**knife ssh 'name:testing' 'sudo chef-node' -x root -P Sanjay@1180**

**Step 5: Test the Environments**
### Accessing Nodes:

Use SSH to access the development and testing nodes individually:
#### For Development Node:

# syntax :
`ssh username@development-node-ip-address`

- Replace `username` with your SSH username and `development-node-ip-address` with the actual IP address or hostname of your development node.
- Example :
**ssh root@192.168.122.62**
#### For Testing Node:

# syntax :

`ssh username@testing-node-ip-address`
- Replace `username` with your SSH username and `testing-node-ip-address` with the actual IP address or hostname of your testing node.
- **ssh root@192.168.122.62**

**Step 6: Document Your Findings**
#### Environment Attributes:
- **Definition**: Chef environments are used to segregate and manage configurations for different stages (Development, Testing, Production).
- **Attributes**: Utilized environment-specific attributes like `web_server_config`, `database_server_ip`, and `ssl_enabled` to tailor configurations based on the environment.
#### Node Assignments:
- **Node Assignment**: Nodes were assigned to specific environments (Development, Testing) using `knife node environment set NODE_NAME ENVIRONMENT_NAME`.
- **Verification**: Confirmed correct assignment via `knife node show NODE_NAME` to ensure nodes were in the expected environments.
#### Cookbook Customization:
- **Cookbook Creation**: Developed a cookbook (`my_webserver_cookbook`) for installing Nginx and customized it to use environment-specific attributes.
- **Adaptations**: Utilized conditionals to access environment-specific attributes and adjusted Nginx configurations accordingly.
#### Deployment Success:
- **Cookbook Upload**: Uploaded `my_webserver_cookbook` to the Chef server.
- **Environment Updates**: Assigned cookbook versions to respective environments (`development` and `testing`) using `knife environment from file`.
- **Node Runs**: Triggered Chef client runs on nodes in each environment to apply the updated configurations.
#### Importance of Environments:
- **Consistency**: Environments ensured consistent configurations across different stages of development (Development, Testing).
- **Isolation**: Isolated configurations prevented unintended changes from affecting production environments prematurely.
- **Testing Ground**: Provided a safe space for validating configurations before deploying to production, minimizing risks.