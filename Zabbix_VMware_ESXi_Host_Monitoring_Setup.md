# Zabbix VMware ESXi Host Monitoring Setup

To begin monitoring VMware ESXi host, you need to create a host.

To do so, navigate to **Configuration > Hosts** and click the **Create host** button.

On the **Host Configuration** screen, you will need to enter the following information:

1. Host Name - The hostname of the ESXi server you wish to monitor.
2. Visible Hostname - This is the friendly name visible on your dashboard and other areas in Zabbix.
3. Groups - Using the **Select** button, choose **Templates/Virtualization**

You will see an option called **Macros** on the navigation bar for the host you are creating. You are going to create three macros as shown below:

Macro | Value | Description
---------|----------|---------
 {$USERNAME} | root | The account used to log into the ESXi web interface.
 {$PASSWORD} | ******** | Using the drop down you can hide the password from view. This is the password the root account uses to access ESXi.
 {$URL} | https://[IP_ADDRESS]/sdk | URL to ESXi web interface.

 Once satisfied, click **Add** to begin monitoring the ESXi host.

It will take up to an hour to begin receiving data from the ESXi host if the Zabbix server defaults were left in place during setup.

From here you can begin adding triggers based on items already being monitored via the built in template.

## Congratulations

You now have a fully operational monitoring solution for your VMware servers for free.
