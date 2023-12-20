
**Part 1: Installation of Chef Test Kitchen**

- Install Chef DK, which includes Test Kitchen.
- code:- `curl https://omnitruck.chef.io/install.sh | sudo bash -s -- -P chefdk`
- This script downloads and installs the Chef DK package.
- Once the installation is complete, you can verify the Test Kitchen installation by running.
- code :-  `kitchen --version`
- This command should display the version number of Test Kitchen if the installation was successful.

**Part 2: Configuration and Testing**

1. **Create the Cookbook Directory**:
	- Open your terminal and navigate to the directory where you want to create the cookbook. Then use the following command.
	- code:- `chef generate cookbook my_cookbook`
	- This command generates a new cookbook named `my_cookbook` with the necessary directory structure.
2. **Create a Recipe to Install Nginx**:
	- Navigate into the `my_cookbook` directory and create a recipe file named `default.rb` inside the `recipes` directory.
	- code:-
`# File: my_cookbook/recipes/default.rb`  
`# Update package repositories (optional step)`

execute 'apt-get update' do
	action :nothing 
end.run_action(:run) if platform?('ubuntu', 'debian')  

`# Install nginx package` 

package 'nginx' do
	action :install
end`
![[Screenshot from 2023-12-18 11-04-32.png]]
- This recipe includes a resource to update the package repositories (useful for some Linux distributions) and then installs the 'nginx' package using the `package` resource.

**Configure Kitchen**

1. **Initialize Test Kitchen**:
	-  Navigate to your cookbook directory (`my_cookbook`) in the terminal and run.
	- code:- `kitchen init`
	- This command initializes Test Kitchen for your cookbook and generates a basic `.kitchen.yml` file with default settings.
2. **Configure `.kitchen.yml` for VirtualBox**
	- Open the `.kitchen.yml` file in a text editor. You'll see a basic configuration that might look similar to this.
	- code :- 
	driver:
	name: <default_driver>`
	- Replace `<default_driver>` with the appropriate driver for your virtualization software. 
	- code :-
			driver:
			   name: vagrant`
	- This configuration specifies the Vagrant driver, which uses VirtualBox as the default provider.
	- You might also want to configure the platform settings. Here's an example for setting up an Ubuntu 20.04 platform.
	- code :-
			driver:
			   name: vagrant
		        platforms:
			                 - name: ubuntu-20.04
			           driver_config:
			                box: ubuntu/20.04`
	- This configuration sets up a platform named `ubuntu-20.04` using the Vagrant driver and specifies the base box as Ubuntu 20.04.

**Create a Test Suite**

1. **Create the Test Suite Directory and Test File**:
	- Navigate to your cookbook directory (`my_cookbook`) and create the test suite directory structure.
	- code :-  `mkdir -p test/integration/default`
	- This command creates the necessary directory structure to hold your test suite.
2. **Create the InSpec Test File**:
	- Inside the `test/integration/default` directory, create a file named `default_spec.rb`.
	- code :-  `touch test/integration/default/default_spec.rb`
	- Open `default_spec.rb` in a text editor.
	- code :-
			`# File: test/integration/default/default_spec.rb`  
			describe package('nginx') do
			   it { should be_installed } 
			end
	- This InSpec test uses the `package` resource to check if the 'nginx' package is installed on the system. It verifies that the package should be installed.
3. **Save the File and Verify the Structure**:
	- Save the changes made to `default_spec.rb`.
	- To verify the directory structure and the presence of the test file, use the `ls` command in the terminal.
	- code :- `ls -R test`
![[Screenshot from 2023-12-18 17-36-41.png]]
- This structure confirms that you've created a test suite (`default`) within the `test/integration` directory and an InSpec test file (`default_spec.rb`) to check the 'nginx' package installation status.

**Create and Converge the Test Environment**

1. **Create the Virtual Machine**:
	- Navigate to your cookbook directory (`my_cookbook`) in the terminal and run.
	- code:- `kitchen create`
	- This command creates the virtual machine instance based on the configuration specified in your `.kitchen.yml` file.
2. **Apply your Cookbook**:
	- Once the virtual machine is created, you can apply your cookbook to it using the following command.
	- code:- `kitchen converge`
	- This command instructs Test Kitchen to converge the virtual machine with the settings and recipes specified in your cookbook. It will install the 'nginx' package according to the recipe you defined earlier.

**Verify the Test**

1. **Run InSpec Tests**:
	- Navigate to your cookbook directory (`my_cookbook`) in the terminal and run the following command.
	- code:- `kitchen verify`
	- This command instructs Test Kitchen to execute the InSpec tests defined in your `default_spec.rb` file within the test suite directory.

**Part 3: Cleanup**

- Navigate to your cookbook directory (`my_cookbook`) in the terminal and run.
- code :- `kitchen destroy`
- This command will remove the virtual machine instance that was created for testing purposes. It cleans up any resources associated with the test environment, ensuring that the virtual machine is completely removed.