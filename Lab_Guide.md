# Lab Notes - FWVMUG June 2020 Enterprise Monitoring with Zabbix Presentation

## Requirements

If you wish to follow along during the presentation, please take a look at these requirements. You will want to have the OVF downloaded and imported into your hypervisor of choice with networking already configured for your environment. You will also need to ensure you can connect via SSH or directly to the VM so that you can enter commands and edit files.

- Internet connection
- An SSH Terminal application such as [PuTTY](https://putty.org/) or [MobaXTerm](https://mobaxterm.mobatek.net/), or [PowerShell 7.x](https://github.com/PowerShell/PowerShell#get-powershell) which now supports SSH natively.
- A modern web browser (Edge,Firefox,Chrome)
- An ESXi 5.5 - 7.0 host and credentials
- [Ubuntu Server 20.04 Focal Fossa OVF](https://www.osboxes.org/ubuntu/)
  - VMware Workstation 15.5, VirtualBox 6.1 or ESXi / vSphere.
  - If you choose this route, be aware you may have to do additional network configuration and firewall editing. This demo is being hosted on my local laptop using Hyper-V and connected to my home network.

-OR-

- [Ubuntu Server 20.04 Focal Fossa ISO](https://ubuntu.com/download/server)

## Suggested Skills

**These are just suggestions, not requirements.**

Some of the suggested skills you should have in order to follow along:

- Basic networking skills
- Basic Linux skills
  - starting / stopping services from command line
  - editing / saving text files

## Zabbix Resources

Here are some Zabbix resources that we will be using during this demo:

- [Download Zabbix](https://www.zabbix.com/download)
- [Zabbix Documentation](https://www.zabbix.com/manuals)
- [Zabbix Forums](https://www.zabbix.com/forum/)
- [Zabbix Training Options](https://www.zabbix.com/training)
- [Zabbix Technical Support Options](https://www.zabbix.com/manuals)
- [Zabbix Professional Services](https://www.zabbix.com/services)

## Demo Plan

The demo should progress as follows:

1. Perform a Zabbix installation on the base OS (Ubuntu 20.04)
2. Perform necessary post installation configuration of Zabbix
3. Setup Host (ESXi) in Zabbix web front end using built-in template
4. Configure a basic trigger based on item in template.
5. Questions

I'm leaving this simple because there's not enough time in the meeting to go over everything,
but this should get you up and running a Zabbix server in about 20 minutes or less with active monitoring of a ESXi host.
