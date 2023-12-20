
**Task 1: Create a Custom Chef Library**
#### Step 1: Create Directory and File

- Navigate to your Chef cookbook's directory and create the necessary folders and files.
- code :- `cd cookbooks/my_cookbook/libraries/custom_library/my_custom_library.rb`
#### Step 2: `my_custom_library.rb`

- Inside the `my_custom_library.rb` file, you can define your custom functions.
- code :-
![[Screenshot from 2023-12-17 17-33-10.png]]

**Task 2: Integrate the Custom Library into a Cookbook**
#### Step 1: Add Dependency and Load Library

1. **Add Dependency**: 
	- In the `metadata.rb` file of your cookbook, specify the dependency on the custom library cookbook.
	- code :-
    `# metadata.rb`  
    `# Add this line to specify the dependency` 
    
    depends 'custom_library'`
#### Step 2: Load Library in Recipe

- In your recipe file (`recipes/default.rb` for instance), load and utilize the functions from the custom library.
- code :-![[Screenshot from 2023-12-17 17-39-16.png]]
#### Usage Explanation:
- The `require_relative` statement loads the custom library file (`my_custom_library.rb`) within the recipe.
- Subsequently, you can use the functions defined in the custom library (`MyCustomLibrary`) to perform tasks like installing packages, managing services, or updating configuration files.

**Task 3: Test the Cookbook and Custom Library**
#### ChefSpec Tests:
- ChefSpec is used to test the correctness of your Chef recipes without actually converging nodes.
1. **Folder Structure**: Create a `spec` directory in your cookbook .
2. **Write ChefSpec Tests:** Inside the `spec` directory, create a file (e.g., `default_spec.rb`) to test the recipe.
3. code :-
![[Screenshot from 2023-12-17 17-42-37.png]]
#### InSpec Tests:

- InSpec tests are used for validating the state of a node after Chef has converged it.
1. **Folder Structure:** inside the `test/integration/default` directory in your cookbook.
2. **Write InSpec Tests:** Inside the test directory, create a file (e.g., `default_test.rb`) to test the applied configuration.
3. code :-
![[Screenshot from 2023-12-17 17-48-09.png]]
#### Running Tests:

- To ensure your tests pass successfully:
1. **ChefSpec**: 
	- Run ChefSpec from the root directory of your cookbook.
	- code:- `chef exec rspec --color --format documentation`
2. **InSpec**:
	- Run InSpec against a node that the cookbook has been converged on.
		- code:- `inspec exec /root/chef-repo/cookbooks/my_cookbook -t ssh://root@192.168.122.62 --password 'Sanjay@1180'`
- These tests will validate that your custom library functions are being called correctly within your cookbook's recipe and that the desired configurations or installations are applied correctly on the target node.

