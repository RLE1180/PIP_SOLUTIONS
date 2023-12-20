
**Step 1: Create a Chef Cookbook**

1. **Create the Cookbook**: 
	- Use the `chef` command-line tool to create the cookbook.
	- code :- `chef generate cookbook web_server`
	- This will generate a new directory named `web_server`, containing the basic structure for your cookbook.
    
2. **Navigate to the Cookbook Directory**:
	- Change your directory to the newly created `web_server` cookbook.
	- code :- `cd web_server`
3. **Test and Execute**: 
	- Once you've set up your cookbook, you can test it locally using `chef-client` or upload it to a Chef Server for deployment.
	- code :- `chef-client --local-mode --runlist 'recipe[web_server::default]'`

**Step 2: Write a Recipe**

- Edit the default recipe in `web_server` cookbook with the following command.
- code :- 
`# Install Nginx package'
package 'nginx' do
action :install
end

`# Start and enable Nginx service

service 'nginx' do
action [:enable, :start]
end

`# Configure Nginx settings using a template

template '/etc/nginx/nginx.conf' do
	source 'nginx.conf.erb'   
	variables(
	     user: node['nginx']['user'],     
	     worker_processes: node['nginx']['worker_processes'],     
	     worker_connections: node['nginx']['worker_connections'],     
	     sites: node['nginx']['sites']     
	     )
	notifies :reload, 'service[nginx]', :delayed
 end`

![[Screenshot from 2023-12-16 18-21-11.png]]
#### Explanation:
1. **Update Package Repositories**: This step ensures the package repositories are up to date, particularly useful for Debian-based systems.
2. **Install Nginx**: Use the `package` resource to install the Nginx package.
3. **Start and Enable Nginx Service**: The `service` resource ensures the Nginx service is started and enabled to start on system boot.
4. **Configure Nginx Settings Using Template**: Use the `template` resource to generate the `nginx.conf` file dynamically. This template might contain the configuration code similar to the example I provided earlier. Ensure you have an `nginx.conf.erb` file in the `templates/default` directory of your cookbook.
5. **Variables**: Pass necessary variables to the template (like `user`, `worker_processes`, `sites`, etc.) from the cookbook's attributes or default settings.
6. **Notification**: The `notifies` directive sends a notification to the Nginx service to reload whenever the configuration file changes.

**Step 3: Create a Template**
#### Step 1: Create the Template File
- Navigate to your "web_server" cookbook directory.
- Inside the `templates/default` directory, create a file named `nginx.conf.erb`.
#### Step 2: Add Placeholder Content

- Here's an example of how you might structure the `nginx.conf.erb` template file:
### `web_server/templates/default/nginx.conf.erb`
-  code :
  user <%= @user %> ;
    events {
         worker_connections <%= @worker_connections %> ;
          }
            http {
                 server {
                          listen <%= @server_ip %>:80;         
                          server_name <%= @server_name %>;          
                          location / {
                                       <%= @custom_message %>
                                                }     
                                    } 
                 }`
#### Step 3: Define Dynamic Content with Placeholders
- `<%= @user %>`: Placeholder for the user running Nginx.
- `<%= @worker_connections %>`: Placeholder for worker connections.
- `<%= @server_ip %>`: Placeholder for the server's IP address.
- `<%= @server_name %>`: Placeholder for the server name.
- `<%= @custom_message %>`: Placeholder for a custom message.

![[Screenshot from 2023-12-17 09-17-39.png]]
**Step 4: Modify the Recipe**

![[Screenshot from 2023-12-17 09-18-04.png]]
#### Explanation:

1. **Update Package Repositories**: Optional step to update package repositories for Debian-based systems.
2. **Install Nginx**: Use the `package` resource to install the Nginx package.
3. **Start and Enable Nginx Service**: The `service` resource ensures the Nginx service is started and enabled to start on system boot.
4. **Configure Nginx Settings Using Template**: The `template` resource generates the `nginx.conf` file dynamically, using the `nginx.conf.erb` template. It passes variables such as `user`, `worker_processes`, `server_ip`, `server_name`, and `custom_message` from the recipe to the template.
5. **Notification**: Sends a notification to the Nginx service to reload whenever the configuration file changes.

**Step 5: Test the Recipe**
#### Bootstrap a Chef Client Node:

1. **Bootstrap the Node** : 
	- Use the `knife bootstrap` command to bootstrap the node.
	- code :- `knife bootstrap 192.168.122.62 -x root --sudo`
	- This command installs the Chef client on the node and connects it to your Chef server.
#### Upload the Cookbook:

1. **Upload the Cookbook**: 
	- Use the `knife cookbook upload` command to upload your `web_server` cookbook to the Chef server.
	- code:- `knife cookbook upload web_server`
#### Apply the Recipe to the Node:

1. **Run Chef Client**: 
	- Apply the `web_server::default` recipe to the node using the `chef-client` command.
	- code :- `knife ssh 'name:chef-node' 'sudo chef-client --runlist "recipe[web_server::default]"' -x root`
	- This sequence of commands will bootstrap the Chef client node, upload your `web_server` cookbook to the Chef server, and apply the `web_server::default` recipe to the specified node, configuring Nginx based on the recipe's instructions.

**Step 7: Customization**
#### Update the Custom Message:

- Now, update the custom message in `recipe/default.rb`
![[Screenshot from 2023-12-17 09-30-33.png]]
#### Reapply the Recipe:
- Upload the updated cookbook to the Chef server.
- code:- `knife cookbook upload web_server`
- Then, reapply the `web_server::default` recipe to the node.
- code :- `knife ssh 'name:chef-node' 'sudo chef-client --runlist "recipe[web_server::default]"' -x root`

**Step 8: Cleanup**
#### Recipe to Remove Nginx Package and Configuration:

- update your existing recipe `recipe/default.rb` within `web_server` cookbook.
- code :-
	package 'nginx' do
	   action :remove 
	end  
	
	nginx_config_files = ['/etc/nginx/nginx.conf', '/etc/nginx/sites-available', '/etc/nginx/sites-enabled']
	nginx_config_files.each do |config_file|
	   file config_file do 
	       action :delete   
	       end 
	end`
- This recipe performs the following actions:
1. Uses the `package` resource to remove the Nginx package.
2. Deletes specific Nginx configuration files.
#### Applying the Recipe to the Node:
 - To execute this recipe on the node.
 - code :-`knife ssh 'name:chef-node' 'sudo chef-client --runlist "recipe[web_server::default]"' -x root`
- Running this Chef recipe will uninstall the Nginx package and remove the specified configuration files from the target node, effectively cleaning up the Nginx installation.
