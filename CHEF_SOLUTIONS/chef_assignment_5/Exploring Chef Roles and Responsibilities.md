
**Task 1: Role Creation**

1. Use the `knife` command to create a new role in your Chef repository.
	- **knife role create webserver**

2. Edit the newly created role to specify the run list, attributes, and any other configuration settings that are relevant to a web server.
	- Ex : 
{
  "name": "webserver",
  "description": "Role for web server",
  "json_class": "Chef::Role",
  "default_attributes": {
    "apache2": {
      "listen_ports": ["80", "443"]
    }
  },
  "run_list": [
    "recipe[apache2]",
    "recipe[php]"
  ],
  "chef_type": "role",
  "env_run_lists": {}
}
![[Screenshot from 2023-12-13 12-42-42 1.png]]

**Task 2: Node Assignment**

1. **Identify the Node:** Locate the node you want to assign the "webserver" role to. You'll need the node's name or IP address.
    
2. **Assign the Role:** Use the `knife node` command to assign the role to the node. Assuming the node name is `chef-node`, use:
    # code :
     `knife node run_list add chef-node 'role[webserver]'`
    - This command adds the "webserver" role to the node's run list.
![[Screenshot from 2023-12-13 12-21-16 2.png]]
    
3. **Run Chef Client on the Node:** Trigger the Chef Client run on the node to apply the configurations specified in the "webserver" role:
    # code :
    `knife ssh "name:chef-node" "sudo chef-client" -x root -P Sanjay@1180`

**Task 3: Chef Run**

1. **SSH into the Node** : Open a terminal and SSH into the node. Replace `your_username` and `node_ip` with your actual SSH username and the IP address of the node.
#### syntax: `ssh your_username@node_ip`
- Ex: **ssh root@192.168.122.62**
	
2. **Run Chef Client** : Once you're logged into the node, initiate the Chef run to apply the configurations specified in the "webserver" role:
    # code
    `sudo chef-client`
    
3. **Monitor the Chef Run:** During the Chef run, observe the output for any errors, warnings, or changes made to the node's configuration. This will display information about the resources being configured, any convergence actions, and any changes applied to bring the node in line with the "webserver" role definition.

**Task 4: Verification**

1. **Check Apache Service Status:** Use the `systemctl` command to check the status of the Apache service:
    # code :
    **sudo systemctl status httpd**
    - This command will show if Apache is active and running. It will also display recent logs and any errors if the service failed to start.
![[Screenshot from 2023-12-14 11-27-27.png]]
    
2. **Verify Listening Ports:** Confirm that Apache is listening on ports 80 and 443. You can check this using the `netstat` command:
    # code :
    **sudo netstat -tuln | grep 'LISTEN\|:80\|:443'**
    - This command filters the output to display ports in listening mode. Ensure that ports 80 and 443 are listed, indicating that Apache is listening on those ports.
![[Screenshot from 2023-12-14 11-27-48.png]]
