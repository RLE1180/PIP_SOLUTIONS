**Task 1: Understanding iptables**

To view the current iptables rules and existing firewall configurations, you can use the `iptables` and `ip6tables` commands as mentioned below.

**For IPv4** : 

`# View the current iptables rules` 
**sudo iptables -L -n -v**

**For IPv6** : 

`# View the current iptables rules for IPv6` 
**sudo ip6tables -L -n -v**

When you run these commands, they will display the existing firewall rules and configurations. The output will include information about chains, rules, policies, and packet counts.
![[Screenshot from 2023-11-07 16-24-13.png]]

![[Screenshot from 2023-11-07 16-24-51.png]]

In the IPv4 section, the INPUT chain allows established and related connections, as well as ICMP traffic. 
In the IPv6 section, the INPUT chain allows all traffic.

**Task 2: Creating a Basic Firewall Rule**

**Step a: Create Firewall Rules** : 

Run the following commands to allow SSH traffic and deny all other incoming traffic:
**sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -j DROP**

These commands add rules to the iptables configuration. The first rule allows incoming SSH traffic on port 22, and the second rule denies all other incoming traffic.

**Step b: Test the Rule** : 

After applying the rules, you can test them by trying to SSH into your Linux system from another machine. If SSH is allowed, you should be able to connect successfully. If not, the connection will be denied.

**Step c: Save the Rules for Persistence:**

To save the iptables rules and make them persistent across reboots, use the following command:

**sudo iptables-save > /etc/iptables/rules.v4**

This command saves the current iptables rules to a file named `rules.v4` in the `/etc/iptables/` directory. These rules will be automatically loaded when your system reboots.

**Task 3: Creating Custom Rules**

1. **Allow Incoming HTTP and HTTPS Traffic** : 

To allow incoming HTTP (port 80) and HTTPS (port 443) traffic, you can use the following commands:

**sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT 
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT**

2. **Allow Outgoing DNS Traffic** : 

To allow outgoing DNS (port 53) traffic, you can use the following commands:

**sudo iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
sudo iptables -A OUTPUT -p tcp --dport 53 -j ACCEPT**

These rules allow outgoing DNS traffic on port 53, both over UDP and TCP, which is commonly used for DNS resolution.

3. **Save the Rules** : 

To save your custom iptables rules and make them persistent, use the following command:

**sudo iptables-save > /etc/iptables/rules.v4**

This command saves the rules to a file named `rules.v4` in the `/etc/iptables/` directory, ensuring that they are loaded automatically when your system reboots.

**Task 4: Monitoring and Troubleshooting**

1. **Monitor Incoming Traffic** : 

Use the `watch` command to monitor incoming traffic in real-time:

**sudo watch -n 1 iptables -vnL**

This command will continuously display the current iptables rules and packet counts, updating every second.

![[Screenshot from 2023-11-07 17-12-40.png]]

2. **Test Incoming Access** : 

Attempt to access your Linux system from another machine using various protocols (e.g., SSH, HTTP).
For example, try to SSH into your Linux system from another machine. If SSH is allowed by your firewall rules, the connection should succeed. Similarly, you can test other services like HTTP.

3. **Troubleshoot Firewall Rules** : 

If you encounter any issues or unexpected behavior, you can troubleshoot by reviewing your rules and checking the `iptables` logs.

1. Review Rules: Check your existing `iptables` rules using the `iptables -L -n -v` command to ensure that your rules are correctly configured. You can use this command to identify any rules that might be blocking the desired traffic.
2. Check iptables Logs: If you have configured `iptables` to log dropped packets, you can review the logs in `/var/log/iptables.log` to see if any packets are being blocked. You can use standard log inspection tools like `cat`, `grep`, or `tail` to view the logs.
Ex :- **sudo cat /var/log/iptables.log**
3. Adjust Rules: If you identify that the rules are blocking desired traffic, you may need to adjust your `iptables` rules accordingly. Modify rules using the `iptables` commands, as needed.
4. Test Again: After adjusting the rules, test the access from another machine again to ensure that it is now permitted.

**Task 5: Cleanup**

To remove all the custom iptables rules and return to the default settings, you can use the following commands:

`# Flush all custom rules` 
**sudo iptables -F 
sudo iptables -X** 

`#Reset policies to default` 
**sudo iptables -P INPUT ACCEPT 
sudo iptables -P FORWARD ACCEPT 
sudo iptables -P OUTPUT ACCEPT** 

`# Save the changes` 
**sudo iptables-save > /etc/iptables/rules.v4**

