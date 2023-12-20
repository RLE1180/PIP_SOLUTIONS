**Create a Custom Resource**:

1. **Create a Cookbook**:
	- creat a new cookbook using chef command.
	- code :- `chef generate cookbook custom_resources_cookbook`
	- it will create a new cookbook called `custom_resources_cookbook`
2. **Create a Directory**:
	- Inside your cookbook, create a directory named `resources`.
3. **Create the Custom Resource File**:
	- In the `resources` directory, create a file named `my_custom_resource.rb` and define the custom resource.
	- code :-
resource_name :my_custom_resource

property :name, String, name_property: true 
property :message, String, default: 'Hello, World!'

action :action1 do
end

action :action2 do
end

![[Screenshot from 2023-12-18 08-30-18.png]]
- This code defines a custom resource called `my_custom_resource` with two properties: `name` and `message`. Two actions, `:action1` and `:action2`, are also defined.

**Define an Action**:

- add `:print` action to  `my_custom_resource.rb` file.
- code:-
	action :print do
	log "Custom Resource Message: #{new_resource.message}"
	end
![[Screenshot from 2023-12-18 08-45-47.png]]

**Create a Recipe**:

- use the `default.rb` file in the `recipes` directory of your cookbook.
- `my_custom_resource` is invoked three times, each with a different action (`:action1`, `:action2`, and `:print`).
- Properties `name` and `message` are specified for each resource instance, providing unique values.
- `:action1` and `:action2` execute the behavior defined within their respective action blocks.
- `:print` uses the `log` resource to print a message to the Chef log containing the value of the `message` property for that resource instance.
![[Screenshot from 2023-12-18 09-03-31.png]]

**Part 2: Testing and Applying the Custom Resource**

**Testing**

- use Test Kitchen along with InSpec to test the functionality of your custom resource. First, create a `.kitchen.yml` file in the root directory of your cookbook:
![[Screenshot from 2023-12-18 09-09-20.png]]
- create a directory for InSpec tests. For example inside the cookbook `custon_resources_cookbook/test/integration/default/default_test.rb`.
- Inside `default_test.rb`, you can write tests to verify the functionality of your custom resource. Here's an example:
![[Screenshot from 2023-12-18 09-18-38.png]]
- After setting up your `.kitchen.yml` and writing your InSpec tests, you can run Test Kitchen to converge and verify your cookbook.
- code :- `kitchen test`
- This command will start the specified platform, converge it with Chef, and then run your InSpec tests to verify the behavior of your custom resource within cookbook's context.

**Applying the Custom Resource**

1. **Create the Wrapper Cookbook**
	- create a new wrapper cookbook.
	- code :- `chef generate cookbook my_wrapper_cookbook`
2. **Modify `metadata.rb`**
	- Update the `metadata.rb` file in your wrapper cookbook to include a dependency of my `custom_resources_cookbook`.
	- code :-
	`# my_wrapper_cookbook/metadata.rb`  
	**depends 'custom_resources_cookbook'**
3. **Modify the Recipe**
	- Within a recipe of your wrapper cookbook, include the recipe(s) from `custom_resources_cookbook`
	- code :- `# my_wrapper_cookbook/recipes/default.rb`  
			**include_recipe 'custom_resources_cookbook::default**
4. **Upload to Chef Server**

1. **Upload the Custom _Resources_Cookbook**
	- code:- `knife cookbook upload custom_resources_cookbook`
2. **Upload the Wrapper Cookbook**
	- After successfully uploading the `custom_resources_cookbook`, proceed to upload the wrapper cookbook.
	- code :- `knife cookbook upload my_wrapper_cookbook`
	- Ensure both cookbooks are correctly uploaded without any errors. Once uploaded, you should be able to use the wrapper cookbook with the included custom resource cookbook on your Chef Server.

**Converge Nodes**

1. **Ensure Chef Configuration**
	- Make sure your Chef workstation is configured properly with the necessary access to your Chef Server or Chef Automate.
2. **Assign the Run List**
	- Assign the run list to the node. You can do this through the Chef Server or by using the `knife node run_list set` command.
	- code :-
	`knife node run_list set chef-node 'recipe[my_wrapper_cookbook::default]'`

![[Screenshot from 2023-12-18 09-33-22.png]]
3. **Run Chef Client on the Node**
	- SSH into the node Then, run the Chef Client to apply the configuration.
	- code:- `chef-client`
	- This command triggers the Chef Client to converge the node, applying the recipes and configurations specified in the run list.
4. **Verify the Custom Resource Functionality**
	- Once the Chef run completes, verify whether the custom resource functionalities are applied as expected on the node. Check the configurations, logs, services, or any other resources managed by your custom resource.
