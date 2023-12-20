
**Step 1: Create a Chef Data Bag**

1. **Navigate to Chef Repository**:
	- Use the `cd` command to navigate to your Chef repository directory.
	- code :-  `cd /root/chef-repo`
2. **Create Data Bag**: 
	- Use the `knife` command to create a new data bag named "users".
	- code:- `knife data bag create users`
	- This command creates an empty data bag named "users" in your Chef repository.

**Step 2: Create Data Bag Items**

- Here's an example structure for the JSON files:

**John's JSON (`john.json`)**:
- code:- 
		{
		   "id": "john",   
		   "username": "john_doe",   
		   "email": "john@example.com",   
		   "roles": ["admin", "developer"] 
		}
![[Screenshot from 2023-12-18 18-08-22.png]]
**Jane's JSON (`jane.json`)**:
- code
		{
		   "id": "jane",   
		   "username": "jane_smith",   
		   "email": "jane@example.com",   
		   "roles": ["analyst"] 
		}
![[Screenshot from 2023-12-18 18-07-55.png]]

- To create these items within the "users" data bag, assuming you're in the Chef repository directory:
1. **Create John's Data Bag Item**:
    - Create a `john.json` file with the content provided above.
    - Upload it to the "users" data bag.
    - code:-  `knife data bag from file users john.json`
        
2. **Create Jane's Data Bag Item**:
    - Create a `jane.json` file with the content provided above.
    - Upload it to the "users" data bag.
    - code :- `knife data bag from file users jane.json`
![[Screenshot from 2023-12-18 18-09-05.png]]

**Step 3: List Data Bag Items**

1. **List Items in "users" Data Bag**: 
	- Use the following command to display all the items within the "users" data bag.
	- code:- `knife data bag show users`
	- This command will output a list of items present in the "users" data bag.
![[Screenshot from 2023-12-18 18-18-23.png]]

**Step 4: Update a Data Bag Item**

1. **Edit John's JSON File (`john.json`):**
    - Locate the `john.json` file within your Chef repository.
    - Update the email address field with the new email. For example.
    - code :-
		    {
		       "id": "john",   
		       "username": "john_doe",   
		       "email": "johndoe@example.com",   
		       "roles": ["admin", "developer"] 
		    }
2. **Upload Updated JSON to "users" Data Bag**: 
	- Use the following command to upload the updated `john.json` file to the "users" data bag.
	- code :- `knife data bag from file users john.json`
	- This command will update the "john" item within the "users" data bag with the modified email address.
![[Screenshot from 2023-12-18 18-22-05.png]]

**Step 5: Delete a Data Bag Item**

- To delete the "jane" Data Bag item within the "users" data bag, use the following command.
- code :- `knife data bag delete users jane`
- This command will remove the "jane" item from the "users" data bag.
![[Screenshot from 2023-12-18 18-33-53.png]]

**Step 6: Use Data Bag Items in a Chef Recipe**

- Chef recipe file named `user_setup.rb`, here's an example of how you might access the "john" data bag item and use its information to create a user account.
- code :-
![[Screenshot from 2023-12-18 18-52-41 1.png]]
Explanation:
- The `data_bag_item` method loads the information stored in the "john" data bag item.
- The `user` resource is used to create or modify a user account based on the retrieved information.
- It sets the username, home directory, shell, and optionally modifies the email.
- It iterates through the roles assigned in the data bag item, allowing you to execute specific tasks or configurations based on those roles.

**Step 7: Run the Chef Recipe**

1. **Associate the Node with the Recipe**:
    - Identify the node you want to apply the recipe to the node `chef-node`.
    - Edit the node's run list to include the recipe. You can do this via `knife` .
    - code :- `knife node run_list add chef-node 'recipe[user_setup]'`
    - This adds the `user_setup` recipe to the run list of `my_node`.
![[Screenshot from 2023-12-18 19-00-52.png]]
2. **Trigger a Chef Client Run**: 
	- On the node `chef-node`, run the Chef client to execute the updated run list and apply the changes.
	- code:- `chef-client`
	- This command triggers the Chef client to fetch and apply the updated configuration, including the `user_setup` recipe.

**Step 8: Cleanup**

- To delete the "john" Data Bag item within the "users" data bag, use the following command.
- code :- `knife data bag delete users john`
- This command will remove the "john" item from the "users" data bag. Always verify the item's deletion by listing the items in the "users" data bag using the `knife data bag show users` command afterward.
![[Screenshot from 2023-12-18 19-12-45.png]]
