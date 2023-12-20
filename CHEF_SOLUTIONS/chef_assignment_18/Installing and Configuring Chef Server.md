
**Step 1: Install Dependencies**

1. **SSH into the Server**
	- code :- `ssh username@server_ip_address`
	- Replace `username` with the appropriate username and `server_ip_address` with the IP address of the server.
	- For Example :- `sudo ssh -p 22 rle1180@192.168.122.247`

![[Screenshot from 2023-12-19 09-24-57.png]]

2. **Update Package List and Upgrade Existing Packages**:
	- code:- `sudo yum update && sudo yum upgrade -y`
	- This command updates the package list and upgrades existing packages on Ubuntu-based systems. 
3. **Install Required Dependencies**:
	- code:- `sudo yum install curl sudo -y`
	- The `curl` package is used for downloading files, and `sudo` provides administrative privileges.

**Step 2: Download and Install Chef Server**

1. Download the Chef Server installation package using wget.
2. Installing the Chef Server Package:

	- Once you've successfully downloaded the package, proceed with the installation based on your operating system:
#### For Ubuntu:
- code:-`sudo dpkg -i chef-server-core_16.12.0-1_amd64.deb`
- This command installs the downloaded `.deb` package using `dpkg` on Ubuntu.
#### For CentOS:
- code:-`sudo rpm -Uvh chef-server-core_16.12.0-1.el7.x86_64.rpm`
- This command installs the downloaded `.rpm` package using `rpm` on CentOS.

**Step 3: Configure Chef Server**

- Running `sudo chef-server-ctl reconfigure` initiates the configuration process for the Chef Server. 
- During the execution of `chef-server-ctl reconfigure`, the Chef Server:
1. Sets up required configurations for services like PostgreSQL, RabbitMQ, nginx, etc.
2. Configures SSL certificates for secure communication.
3. Creates administrative users and organizations.
4. Initializes databases and configures services to work together.

**Step 4: Create an Administrator User**

1. **Command to Create an Administrator User**:

**Create an administrator user for Chef Server**:

- code:-
`sudo chef-server-ctl user-create admin Admin User admin@example.com 'your_password_here' --filename admin.pem`
**explanation**:
- `sudo chef-server-ctl user-create`: Initiates the creation of a new user.
- `admin`: Username of the new administrator user.
- `Admin User`: Display name for the administrator user.
- `admin@example.com`: Email address associated with the administrator user.
- `'your_password_here'`: Replace this placeholder with a strong password for the administrator user.
- `--filename admin.pem`: Specifies the filename for the private key associated with this user. The generated private key will be saved as `admin.pem`.
- For Example:-
`sudo chef-server-ctl user-create sanjay sanjay kumar sanjay@example.com 'Sanjay@1180' --filename sanjay.pem`
- It will create the administrator user with the specified details and a corresponding private key file (`sanjay.pem`).

Create an organization:

2. Command to Create an Organization:

- code:-
`sudo chef-server-ctl org-create myorg 'My Organization' --association_user admin --filename myorg-validator.pem`
**Explanation**
- `sudo chef-server-ctl org-create`: Initiates the creation of a new organization.
- `myorg`: Name of the new organization.
- `'My Organization'`: Display name for the organization.
- `--association_user admin`: Associates the organization with the user `admin` created earlier.
- `--filename myorg-validator.pem`: Specifies the filename for the validator key associated with this organization. The generated validator key will be saved as `myorg-validator.pem`.
- For Example :
`sudo chef-server-ctl org-create acharya 'acharya institue' --association_user sanjay --filename acharya-validator.pem`
- It will create an organization with the specified details and a corresponding private key file (`acharya-validator.pem`).

![[Screenshot from 2023-12-19 11-30-45.png]]

**Step 5: Access the Chef Server Web UI**

- Open a web browser and enter the URL based on your Chef Server's IP address or hostname.
- code:- `https://<your-server-ip-or-hostname>`
- Replace `<your-server-ip-or-hostname>` with the actual IP address or hostname of your Chef Server.
- For Example : https://192.168.122.247
![[Screenshot from 2023-12-19 11-39-20.png]]