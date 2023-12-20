#### Step 1: Create a new cookbook named "custom_providers"

- Use knife command to create a new cookbook named "custom_providers".
- code :- `chef generate cookbook custom_providers`
#### Step 2: Create a Custom Provider

- Inside the `custom_providers` cookbook, navigate to `cookbooks/custom_providers/resources` directory.
- Create a file named `my_custom_provider.rb` 
- code :-
![[Screenshot from 2023-12-17 18-25-32.png]]
#### Step 3: Implement the Action

- In the same directory (`cookbooks/custom_providers/providers`), create a file named `my_custom_provider.rb` to define the actions for your provider.
- code :-
![[Screenshot from 2023-12-17 18-26-08 1.png]]
#### Step 4: Write Documentation

- Create a README or documentation file in your cookbook directory (`cookbooks/custom_providers`) detailing:
- How to use the custom provider (`my_custom_provider`)
- Explanation of supported properties (like `file_path`)
- Sample usage or code snippets

Here's an example:
![[Screenshot from 2023-12-17 18-25-58 1.png]]

**Task 2: Modify an Existing Provider**
#### Step 1: Locate and Modify the Provider

- Navigate to the `providers` directory of your "apache" cookbook and locate the provider file associated with the `site` resource. For instance, let's assume the file is named `apache_site.rb`.
#### Step 2: Extend the Functionality

- Open `apache_site.rb` and add a new action or enhance an existing action within the provider. 
- code :-
action :enable do
`# ...existing code to enable the site
`# Extend the enable action to restart Apache after enabling the site
service 'apache2' do
   service_name 'httpd'
   action :restart
 end
end
#### Step 3: Document the Changes
- Update the documentation for the `site` resource within the cookbook's README or documentation file. 

**Task 3: Write a Recipe Using Custom Providers**
#### Step 1: Create the Cookbook

- Use knife command to generate a new cookbook named "custom_provider_recipe":
- code :- `chef generate cookbook custom_provider_recipe`
#### Step 2: Write the Recipe

- Create a recipe file, for instance, `recipes/default.rb`, within the `custom_provider_recipe` cookbook.
- code :-
![[Screenshot from 2023-12-17 19-02-43.png]]

**Task 4: Testing and Validation**
#### Step 1: Upload Cookbooks to Chef Server

- Using the `knife` command to upload the `custom_providers` and `custom_provider_recipe` cookbooks to your Chef server.
- code :-
`knife cookbook upload custom_providers`
`knife cookbook upload custom_provider_recipe`

![[Screenshot from 2023-12-17 18-54-00.png]]
#### Step 2: Create a Test Environment

- Create a test environment using `knife` or the Chef management console.
- code :- `knife environment create test_environment`

![[Screenshot from 2023-12-17 18-56-08.png]]
- Associate the `custom_provider_recipe` cookbook with this test environment.
- code :- `knife environment cookbook add test_environment custom_provider_recipe`
#### Step 3: Bootstrap a Test Node

- Bootstrap a test node with Chef using `knife`.
- code :-
`knife bootstrap 192.168.122.62 --ssh-user root --sudo --identity-file ~/.ssh/private_key.pem --node-name test-node --run-list 'recipe[custom_provider_recipe]'`
- This command will connect to the test node via SSH, install the Chef client, and apply the `custom_provider_recipe` recipe to the node.
#### Step 4: Verify Functionality on the Test Node

- Log in to the test node and verify that the custom provider action and modified provider action work as expected:
- Check if the file specified in the `custom_provider_recipe` recipe (`/path/to/my/custom/file.txt`) has been created.
- Verify if the Apache site specified in the `apache_site` resource from the `custom_provider_recipe` recipe has been enabled and restarted.
- This verification ensures that both the custom provider and the modified provider's actions are functioning correctly on the test node.
