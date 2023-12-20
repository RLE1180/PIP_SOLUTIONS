
												**Exploring Systemctl Commands**


**Task 1: Research and Study the basics of systemctl and systemd services**

'systemctl' is a command-line utility used in unix-like operating systems,primarily linux, to manage and control services and units in the system's init system,which is typically systemd.
Systemd is a system and service manager for Linux operating systems that is responsible for initializing and managing system processes and services during the boot process and while the system is running.

**Basics of systemctl and systemd services** : 

1. **Unit Types**: Systemd uses the concept of "units" to manage various aspects of the system. There are different types of units, including services, sockets, devices, timers, etc.
2. **Service Units**: A service unit is a type of systemd unit used to define and control a system service. It contains information about how a service should be started, stopped, and managed. Service units are typically used for daemons and background services.

3. 'systemctl commands' : 
	- `systemctl start <service_name>`: Starts a service.
	- `systemctl stop <service_name>`: Stops a service.
	- `systemctl restart <service_name>`: Restarts a service.
	- `systemctl enable <service>`: Enables a service to start on boot.
	-  `systemctl disable <service>`: Disables a service from starting on boot.
	- `systemctl status <service>`: Displays the status and information about a service.
	- `systemctl list-units`: Lists all active units.

4. **Service Unit Files**: Service units are defined using unit files typically stored in the '/etc/systemd/system/' directory or '/usr/lib/systemd/system/'. These unit files contain configuration information for the service, including its executable path, dependencies, and other settings.
5. **Logging**: Systemd provides its own logging system, `journald`, which collects and stores log messages from services. You can use `journalctl` to view and filter log entries.

**Difference between System Services and User Services:**

1. **System Services**:
    
    - System services are managed by the system's init system (typically systemd).
    - They run with system-level permissions.
    - System services are started at boot and can be managed by the system administrator.
    - Their unit files are usually stored in `/etc/systemd/system/`.
2. **User Services**:
    
    - User services are managed by systemd as well but are specific to a user session.
    - They run with the user's permissions.
    - User services can be started when a user logs in and stopped when the user logs out.
    - Their unit files are typically stored in `~/.config/systemd/user/`.

**Task 2: Exploring Basic `systemctl` Commands**

1. `systemctl list-units`:
	- lists all loaded units, including services, sockets, targets, and more.
	- This command will provide a list of all active units with their current state, as well as additional information like descriptions and whether they are enabled or disabled.

![[Screenshot from 2023-10-31 14-00-51.png]]
2. `systemctl list-units --type=service`:
	- lists only service unnits.
	- This command will display a list of active service units, similar to the previous command, but limited to service units only. It provides information about the current state, descriptions, and whether they are enabled or disabled.

![[Screenshot from 2023-10-31 15-46-37.png]]
3. `systemctl status <service-name>`:
	- it displays the status of a specific service.
	- Running this command with a specific service name will provide detailed information about that service. It will show its current status, its associated unit file, a description, and recent log entries related to the service. You'll see whether it's active, enabled, and more.
	Ex:- **sudo systemctl status elasticsearch.service**
![[Screenshot from 2023-10-31 15-49-49.png]]
4. `sudo systemctl start <service-name>`:
	- it will starts a service
	- If successful, this command will start the specified service and won't produce any output unless there are issues or errors. If there are problems starting the service, error messages will be displayed.
	**Ex:- sudo systemctl start elasticsearch.service**
![[Screenshot from 2023-10-31 15-56-31.png]]
5. `sudo systemctl stop <service-name>`:
	- this command stops a service.
	- If successful, this command will stop the specified service and won't produce any output unless there are issues or errors. If there are problems stopping the service, error messages will be displayed.
	**Ex:- sudo systemctl stop elasticsearch.service.**
![[Screenshot from 2023-10-31 16-04-51.png]]

**Task 3: Enabling and Disabling Services**

1. `sudo systemctl enable <service-name>`:
	- This command enables a service to start automatically at boot time. When you enable a service, systemd will create symbolic links to the service unit file in the appropriate target directories to ensure the service is started during system boot.
	- Effect of the service specified by `<service-name>` will be configured to start automatically when the system boots up. You won't see any output unless there's an error, in which case you'll receive an error message.
	**Ex:- sudo systemctl enable elasticsearch.service**
![[Screenshot from 2023-10-31 16-12-41.png]]
2. `sudo systemctl disable <service-name>`:
	- This command disables a service from starting automatically at boot time. It removes the symbolic links that were created when the service was enabled.
	- Effect of the service specified by `<service-name>` will be configured to start automatically when the system boots up. You won't see any output unless there's an error, in which case you'll receive an error message.
	**Ex:- sudo systemctl disable elasticsearch.service**
![[Screenshot from 2023-10-31 16-15-21.png]]

**Task 4: Managing System Services**

Let's take the example replace `<service-name>` with the actual name of the service. In this case we'll use SSH which is the service name on some Linux distributions.

1. **start the SSH service**
	- To start the SSH service,use the following command : 
		**sudo systemctl start ssh**
	This command will start the ssh service.you may not see any output if it starts 
	successfully.
2. **Stop the SSH service**
	- To stop the SSH service,use the following command :
	  **sudo systemctl stop ssh**
	This command will stop the ssh service,you may not see any output if it stops successfully.
3. **Enable SSH to start on Boot** :
	- To enable the ssh service to start automatically on boot, use the following command:
		**sudo systemctl enable ssh**
	This command creates symbolic links to ensure that ssh will start on system boot. You should see a message indicating that the service has been enabled.
4. **Disable ssh from Starting on Boot**:
	- To disable ssh from starting automatically on boot, use the following command:
		**sudo systemctl disable ssh**
	This command removes the symbolic links that enable ssh to start on boot. You should see a message indicating that the service has been disabled.
5. **check the status of ssh**
	- To check the status of the ssh,use the following command:
		**sudo systemctl status ssh**
	This command will display detailed information about the current status of the ssh service, including whether it's running or not.
![[Screenshot from 2023-10-31 16-56-46.png]]




  




