**Step1 : Handler Creation**

- Let's create a Ruby script named `custom_handler.rb` and place it in the `files/default` directory.
- code
![[Screenshot from 2023-12-17 11-19-53.png]]
- This script defines a `MyCustomHandler` class inherited from `Chef::Handler` and implements the `report` method. In this example, it logs a message indicating the successful completion of the Chef run to a log file (`/var/log/chef/custom_handler.log`).
- Now, within the cookbook, we want to call and enable this handler in the recipe we can do this using the `chef_handler` resource.
- code:-
`# In your recipe`
chef_handler 'MyCustomHandler' do
source 'custom_handler.rb'
supports report: true
action :enable
end`
- This code uses the `chef_handler` resource to enable the `MyCustomHandler` by specifying the source as `custom_handler.rb` from the `files/default` directory within your cookbook.

**Step2:Recipe Modification**

- we have a recipe (`web_server::install_package`) that installs a package, let's add the custom handler activation to trigger after the package installation.
- code :-
![[Screenshot from 2023-12-17 11-27-01.png]]
In this example:
1. **Package Installation**:
    - The recipe installs a package named `'your_package_name'` using the `package` resource.
2. **Custom Handler Activation**:
    - `chef_handler` resource is used to enable the `MyCustomHandler`.
    - `subscribes` is used to trigger the handler immediately after the specified package (`'your_package_nam'`) is converged.

**Step3:Chef-client Run**

1. Run the Chef-client:
	- Run the Chef-client with the specified recipe that includes the custom handler. For example:
	- code:- `sudo chef-client -o 'recipe[web_server::install_package]'`

2. Observing Handler's Action:
	- During the Chef-client run, as the recipe converges and installs the package, the custom handler (`MyCustomHandler`) should be triggered immediately after the package installation is completed.

3. Verification:
	- Inspect the log content to verify if the message from the handler appears. If everything worked as expected, you should see the log entry indicating the completion of the Chef run at the specific point triggered by the package installation.

**Step4:Documentation**
#### Creating Custom Chef Handler:
#### 1. Setting Up the Handler File:
1. **Create the Handler Script**:
     - Create a Ruby script for the custom handler and place it in the `/files/default/` directory of your cookbook.
    - The handler script (`custom_handler.rb`) should contain the class definition inheriting from `Chef::Handler` and implement the necessary methods.
2. **Activation in Recipe**:
    - Modify an existing recipe to include a call to the custom handler at a specific point during the Chef-client run.
    - Use the `chef_handler` resource in the recipe to enable the custom handler after a specific resource converges.
#### 2. Error Handling in the Handler:
1. **Error Handling Implementation:**
    - Implement error handling within the custom handler using `begin`, `rescue`, and `ensure` blocks.
    - Catch potential exceptions (e.g., `StandardError`) and define appropriate actions to handle them gracefully.

#### 3. Execution and Verification:
1. **Run the Chef-client**:
    - Execute the modified recipe using the Chef Infra Client on the virtual machine.
    - Ensure the Chef-client run demonstrates the execution of the custom handler.
2. **Observing Handler's Action**:
    - Verify the successful execution of the Chef-client run.
    - Check logs or output files to confirm the actions performed by the custom handler.