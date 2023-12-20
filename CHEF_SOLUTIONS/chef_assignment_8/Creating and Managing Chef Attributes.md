#### Understanding Attributes:

1. **Node Attributes:** Nodes in Chef have attributes that describe their current state. These could be default attributes (set in recipes), automatic attributes (gathered by Chef), or normal attributes (user-defined attributes).
    
2. **Cookbook Attributes:** These are set in cookbooks to define default values for attributes that might change across different nodes.
    
#### Creating and Managing Attributes:

1. **Cookbook Attributes:**
    - **Define Attribute Files** : Within your cookbook, create an attributes file, conventionally named `default.rb` within the `attributes` directory.
    - **Set Default Values** : Define default values for attributes using the `default` keyword.
    - code :-  `default['cookbook_name']['attribute_name'] = 'value'`

2. **Overriding Attributes** :
	- **Roles** : Define attributes specific to certain roles. Assign roles to nodes, and those nodes will inherit the attributes set in the role.
	- code :- 
        default_attributes(
           'cookbook_name' => {
                'attribute_name' => 'new_value' 
                  }
                   )
        
    - to set attributes specific to a particular environment.
    - code :- `knife node attribute set node_name attribute_name attribute_value`

3. **Accessing Attributes:**
    - Within Recipes: Access attributes using the `node` object.
    - code :- `node['cookbook_name']['attribute_name']`

**Task 1: Attribute Definition**

1. Create a New Cookbook:
	- code :- `chef generate cookbook attribute_demo`
	- This command will generate a new Chef cookbook named `attribute_demo`.

2. Create the Attribute File:
	- Navigate to the `attributes` directory within your `attribute_demo` cookbook.
	- code :- `cd attribute_demo/attributes`
	- Create a file named `default.rb`.
	- code :- `touch default.rb`

3. Define Attributes:
	- Open the `default.rb` file you've created and define the attributes as requested.
	- code :-
`# attribute_demo/attributes/default.rb'
'# Attributes for webserver configuration'

default['webserver']['port'] = 80
default['webserver']['document_root'] = '/var/www/html'

'# Attributes for database configuration' 

default['database']['host'] = 'localhost'
default['database']['username'] = 'db_user'
default['database']['password'] = 'db_password'

![[Screenshot from 2023-12-16 08-34-32.png]]

4. Verification :
	- To verify that the attributes are set correctly, you can run.
	- code :- `chef-client --local-mode --override-runlist attribute_demo::default`
	- This command executes the Chef client in local mode for the `attribute_demo::default` recipe, applying the defaults specified in the attributes file.

**Task 2: Recipe Implementation**

1. Create the "webserver" Recipe:
	- Navigate to your cookbook's directory.
	- code :- `cd attribute_demo`
	- Create a recipe named `webserver.rb`.
	- code :-  `touch recipes/webserver.rb`

2. Use Attributes to Configure the Web Server:
	- Edit the `webserver.rb` recipe to use the attributes for configuring the web server.
	- code :- 
![[Screenshot from 2023-12-16 09-21-48.png]]

3. Create a Template:
	- we need a template file referenced in the recipe to set up the nginx configuration.
	- Create a template file named `default.erb` in the `templates/default` directory.
	- code :- 
	**mkdir -p templates/default**
     **touch templates/default/default.erb**
     - Edit the `default.erb` template file with the nginx configuration.
     - code :-
	`# attribute_demo/templates/default/default.erb'
	  <VirtualHost *:<%= @port %>>
	     DocumentRoot "<%= @document_root %>" 
	   </VirtualHost>

4. Usage
	- To apply this recipe.
	- Update your `attribute_demo` cookbook's `metadata.rb` file to include the `webserver` recipe.
![[Screenshot from 2023-12-16 09-24-18.png]]

- Then, you can run the following command.
- code :-`chef-client --local-mode --override-runlist attribute_demo::webserver`
- This will execute the `webserver` recipe within your `attribute_demo` cookbook, utilizing the attributes to configure the web server according to the specified port and document root values.

**Task 3: Node Configuration**

1. **Bootstrap the Node** : 
	- Use the `knife bootstrap` command from your Chef Workstation to initiate the bootstrapping process.
	- code :- `knife bootstrap 192.168.122.62 -x root -P Sanjay@1180 --sudo -r 'recipe[attribute_demo::webserver]'`

**Task 4: Attribute Overrides**

1. Create a New Role:
	- Using Chef Workstation, create a new role named "webserver_node".
	- code :-  `knife role create webserver_node`

2. Override Attributes in the Role:
	- Edit the `webserver_node.json` file to define the role and override the attributes for the `webserver` recipe.
	- code :- 
	![[Screenshot from 2023-12-16 10-33-24.png]]

3. Apply the Role to the Node:
	- Associate the `webserver_node` role with the Chef node created earlier task using the following command.
	- code :- `knife node run_list add chef-node 'role[webserver_node]'`

![[Screenshot from 2023-12-16 10-34-56.png]]

4. Verify Attribute Override:

	- SSH into the node and verify that the web server is using the overridden attribute values.
	- **Check Web Server Configuration** :
	- For nginx:
	- code :- `cat /etc/nginx/nginx.conf`

**Task 5: Reporting**

- **Task 1: Attribute Definition**
    - Created a `attribute_demo` cookbook.
    - Defined attributes in `default.rb` related to web server and database configurations.
- **Task 2: Web Server Recipe**
    - Created a `webserver` recipe within the `attribute_demo` cookbook.
    - Utilized attributes to configure the web server Nginx using `node['webserver']` attributes for port and document root settings.
- **Task 3: Node Bootstrap and Role Assignment**
    - Bootstrapped a Chef node and assigned the `attribute_demo::webserver` recipe during the bootstrap process.
    - Created a `webserver_node` role overriding `['webserver']['port']` and `['webserver']['document_root']`.
    - Applied the role to the Chef node.
- **Task 4: Attribute Verification**
    - Verified the web server configuration on the node to ensure it used the overridden attribute values.
#### Challenges and Resolutions:

- **Challenge**  : Incorrect run list or role definition causing attribute override not to take effect.
    - **Resolution** : Verified the run list and role definition to ensure proper assignment of the `webserver_node` role.
- **Challenge** : Verification discrepancies in the web server's actual configuration and expected attribute values.
    - **Resolution** : Checked the configuration files of the web server (e.g., `/etc/nginx/nginc.conf`) to validate attribute overrides.
