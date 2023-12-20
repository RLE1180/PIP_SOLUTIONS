
**Task 1: Create a Chef Node**

1. **Access Chef Workstation:** Ensure you have terminal or command line access to your Chef Workstation where the `knife` command is available.
    
2. **Bootstrap the Node:** Use the `knife bootstrap` command to bootstrap the target server. Replace `NODE_HOSTNAME_OR_IP`, `SSH_USER`, and `SSH_KEY_PATH` with your specific details:
    
    # syntax:
	'knife bootstrap NODE_HOSTNAME_OR_IP -x SSH_USER --sudo -i SSH_KEY_PATH --node-name NODE_NAME'

    - `NODE_HOSTNAME_OR_IP`: The IP address or hostname of your target node.
    - `SSH_USER`: The SSH username used to connect to the node.
    - `SSH_KEY_PATH`: The path to the SSH private key used for authentication.
    - `NODE_NAME`: Desired name for your Chef node.

    Example: code
    **knife bootstrap 192.168.122.246 -x root --sudo -i ~/.ssh/private_key.pem --node-name client**
- This command will install the necessary Chef components on the target server and configure it to communicate with your Chef Server.
    
    #### Verify Node Creation

1. **Check Nodes on Chef Server** :  After the bootstrap process completes successfully, verify that the node appears in the list of nodes on your Chef Server.
    
    - Use the following command to list nodes:
    # code :
    **knife node list**
    - If successful, you should see the node you just added (`client` in the example) in the output.
    ![[Screenshot from 2023-12-15 11-03-54 2.png]]
#### Additional Checks

- **Connectivity Test:** You can also test the connectivity to the node using `knife ssh`. For instance:
    # code :
    **knife ssh 'name:client' 'sudo chef-client' -x root -i ~/.ssh/private_key.pem**
    
    ![[Screenshot from 2023-12-15 11-06-56.png]]

**Task 2: Write and Apply a Chef Recipe**

1. **Create the Recipe File** : Open your text editor and create a new file named `apache-recipe.rb` in the `cookbooks` directory on Chef Workstation.
    
2. **Write the Recipe:** Add the following code to the `apache-recipe.rb` file:
    
    # code :
    
    **# apache-recipe.rb
    
	package 'httpd' do
	   action :install 
	end  
	service 'httpd' do
	   action [:enable, :start] 
	end 
    
    - This simple recipe uses Chef's resources to install the `httpd` package and ensure that the `httpd` service is enabled and started.
#### Upload the Recipe to Chef Server

1. **Upload the Cookboo** : Use the `knife cookbook upload` command to upload the cookbook to your Chef Server.
Ex:- **knife cookbook upload apache-cookbook**
- This command uploads the cookbook named `apache-cookbook` to your Chef Server
    
#### Assign the Recipe to the Node

1. Use the `knife bootstarp` command to assign the `httpd-install` recipe to your Chef node:
    # code:
    **knife bootstrap chef-node -P Sanjay@1180 -U root --sudo --use-sudo-password -r "recipe[apache-cookbook::apache-recipe]".
    
#### Verify Nginx Installation

1. **Check httpd Status** : After the Chef-client run completes, verify that httpd is installed and running on your node by accessing the node or using commands like:
    # code:
    **systemctl status httpd**
    - This command should show the status of the httpd service, indicating whether it's active and running.
![[Screenshot from 2023-12-15 11-53-42.png]]

**Task 3: Node Management**
#### Modify the Recipe

- Edit the `apache-recipe.rb` recipe in your Chef workstation's `cookbooks` directory:
# code :

'# install_httpd.rb'
'# Install httpd package'

**package 'httpd' do
	action :install
end

'# Start and enable the httpd service'

**service 'httpd' do
	action [:enable, :start]
end

'# Install vim package' 
package 'vim' do
	action :install
end  
'# Modify configuration'

welcome page template '/var/www/html/index.html' do
	source 'index.html.erb'
	mode '0644'   
	owner 'root'   
	group 'root'
end**
#### Create an index.html.erb Template

- Create a template file named `index.html.erb` in the `templates/default` directory within your cookbook. This template will replace the default `index.html` file.
- Add the HTML content to this template file:
![[Screenshot from 2023-12-15 12-55-11 1.png]]
#### Upload Updated Recipe

Upload the modified cookbook to your Chef Server:
# code :
**knife cookbook upload apache-cookbook**
- This command updates the `apache-cookbook` cookbook on your Chef Server.
#### Run Chef-Client on the Node

- SSH into your Chef node and run the Chef-client to apply the updated configuration:
# code :
**knife ssh 'name:chef-node' 'sudo chef-client' -x root -i ~/.ssh/private_key.pem**

**Task 4: Reporting and Monitoring**

1. Use Knife Commands: You can also retrieve some information using `knife` commands:
	- To view the latest run's status and summary:
	- code :-
	- **knife node show chef-node --run-list**
![[Screenshot from 2023-12-15 13-20-05.png]]

**Task 5: Node Decommissioning**
#### Decommission the Chef Node

- Run the following `knife node delete` command to remove the node from the Chef Server:
- code :-
**knife node delete client**
#### Verify Node Removal

- After executing the deletion command, verify that the node has been successfully removed from the Chef Server's list of nodes.
- Use the `knife node list` command to list all nodes:
- code :-
**knife node list**
- Ensure that the node removed no longer appears in the output list. If it doesn't appear, it confirms the successful removal of the node from the Chef Server.

![[Screenshot from 2023-12-15 13-35-25.png]]