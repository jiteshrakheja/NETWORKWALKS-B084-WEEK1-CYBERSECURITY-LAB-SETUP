</div>

<p align="center">
  <img src="https://img.shields.io/badge/Focus-Cybersecurity-404040?style=flat-square&labelColor=C00000" alt="Cybersecurity" />
  <img src="https://img.shields.io/badge/VirtualBox-7.2-0070C0?style=flat-square&labelColor=000000" alt="VirtualBox 7.2" />
  <img src="https://img.shields.io/badge/Kali%20Linux-2026.3-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" alt="Kali Linux 2026.3" />
  <img src="https://img.shields.io/badge/Networking-NAT%20Network-238F89?style=flat-square&labelColor=000000" alt="NAT Network" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-Lab-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" alt="Penetration testing lab" />
  <img src="https://img.shields.io/badge/Platform-Windows%2011-0078D4?style=flat-square&labelColor=000000" alt="Windows 11" />
</p>

📌 Project Overview
This project documents my setup of a virtual cybersecurity laboratory using Oracle VirtualBox and Kali Linux. The goal is to build a controlled environment for learning Linux, virtual networking, reconnaissance, network scanning, vulnerability assessment, packet analysis, and other authorized security-testing techniques.

The lab uses a private VirtualBox NAT Network so that additional virtual machines can be added for controlled, multi-machine exercises.
🎯 Objectives
- Install and configure VirtualBox.
- Import Kali Linux as a virtual machine.
- Create a dedicated NAT Network for the lab.
- Configure and verify Kali Linux network connectivity.
- Set up a consistent IP address for the Kali VM.
- Test gateway connectivity, internet access, and DNS resolution.
- Create a clean VM snapshot for recovery.
- Document the configuration, tests, and troubleshooting steps.
- Prepare the environment for future cybersecurity projects.
🛡️ Purpose and Scope
This lab is intended for cybersecurity education and authorized testing. Planned and potential exercises include:
- Network reconnaissance and host discovery
- Port scanning
- Vulnerability assessment
- Packet capture and analysis
- Web application security testing
- Safe exploitation practice in intentionally vulnerable targets
- Security-tool experimentation
Only test systems that I own or have explicit permission to assess.
🏗️ Lab Architecture

The current lab contains a Windows host running VirtualBox and a Kali Linux virtual machine connected to a NAT Network. Additional target machines can be connected to the same lab network later.
⚙️ Lab Configuration
Component	Configuration
Host OS	Windows 11
Host RAM	16 GB
Processor	Intel Core i5
Hypervisor	Oracle VirtualBox 7.2
Security OS	Kali Linux 2026.3 (verify exact installed release)
Kali VM RAM	4096 MB
Virtual network	NAT Network
Network address	10.0.0.0/24
Kali IPv4 address	10.0.0.2/24 (verify in the VM)
Default gateway	10.0.0.1 (verify in VirtualBox and Kali)
DNS server	8.8.8.8 (verify actual resolver configuration)
Planned VM address range	10.0.0.3–10.0.0.99 (subject to DHCP-range checks)


🧰 Tools Used
- 7-Zip — extracting the downloaded virtual-machine archive when required
- Oracle VirtualBox — creating and managing virtual machines and virtual networks
- Kali Linux — Linux-based security-testing environment
- NetworkManager (nmcli) — inspecting and managing network connections
- Nmap — network discovery and port-scanning tool
🪜 Setup Procedure
Step 1: Install 7-Zip
I installed 7-Zip to extract the Kali Linux virtual-machine package if it was provided as a .7z archive.
Step 2: Install VirtualBox
I installed Oracle VirtualBox to host and manage the Kali Linux virtual machine.
Step 3: Create the NAT Network
I created a dedicated NAT Network in VirtualBox with the following intended settings:
Network name: NatNetwork
IPv4 prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled

A NAT Network allows virtual machines connected to that network to communicate with one another and generally provides outbound connectivity through network address translation. This is useful for a multi-VM practice lab.
Configuration note: Confirm the NAT Network's actual IPv4 prefix, DHCP status, and DHCP address pool in VirtualBox's Network Manager. Ensure any manually assigned IP address is outside the DHCP pool or is otherwise reserved to avoid address conflicts.

Step 4: Import and Configure Kali Linux
I downloaded Kali Linux from the official Kali Linux website and imported the virtual machine into VirtualBox.

The intended VM network adapter configuration is:
Adapter:       Adapter 1
Attached to:   NAT Network
Network name:  NatNetwork
Adapter type:  Intel PRO/1000 MT Desktop
Memory:        4096 MB

A shared folder was also configured to transfer required files between Windows and the Kali VM. Shared folders should only contain files that need to be exchanged with the guest; avoid exposing sensitive host files unnecessarily.
Step 5: Configure the Kali Linux Network
I checked the Kali network configuration and set up a consistent IPv4 address for the VM.

Intended configuration:
IPv4 address:  10.0.0.2
Prefix:        /24
Subnet mask:   255.255.255.0
Gateway:       10.0.0.1
DNS server:    8.8.8.8

A consistent address makes it easier to identify the Kali VM during later lab exercises. The address, gateway, and DNS values should match the actual VirtualBox NAT Network configuration and the active Kali connection profile.
Step 6: Create a Clean VM Snapshot
After the initial setup, I created a VirtualBox snapshot named:
Clean Kali - Network Setup

This snapshot is intended to serve as a clean recovery point. Before risky or experimental exercises, I can create additional snapshots so that changes can be rolled back without rebuilding the environment.
🔎 Lab Verification
The following checks were used to validate the setup. Run them again after changes to the VM or network configuration.
Check	Command	What to verify
Inspect interfaces and addresses	ip a	The active interface has the expected IPv4 address
Inspect routes	ip route	The default route points to the configured gateway, if applicable
Test gateway reachability	ping -c 4 10.0.0.1	Replies are received, if ICMP is allowed and the gateway address is correct
Test external IP connectivity	ping -c 4 8.8.8.8	Replies indicate ICMP reachability to that destination
Test DNS resolution	nslookup networkwalks.com	The domain resolves through the configured resolver
Check Nmap installation	nmap --version	Nmap version information is displayed
Check snapshot recovery	Restore the snapshot, then run ip a	The VM returns to the saved baseline state


1. Check the Kali VM IP address
Run:
ip a
The expected address for the lab is 10.0.0.2/24, assigned to the active network interface.

Note: ip a displays interface addresses and flags; it does not, by itself, prove that an address is static. Check the active NetworkManager connection profile to confirm whether IPv4 configuration uses manual addressing or DHCP.
2. Test gateway and internet connectivity
Run:
ping -c 4 10.0.0.1
ping -c 4 8.8.8.8

Successful replies indicate that the destinations responded to ICMP at the time of testing. A failed ping does not always mean all network connectivity is unavailable, because ICMP may be filtered.
3. Test DNS resolution and Nmap
Run:
nslookup networkwalks.com
nmap --version

The DNS test should return an address for the domain when resolution is working. The Nmap command should display the installed version.
Test configuration
Expected Kali address: 10.0.0.2/24
Expected gateway:      10.0.0.1
Configured DNS:        8.8.8.8
These are the documented intended values; verify them against the actual VM and VirtualBox settings before relying on them.
4. Verify snapshot recovery
I restored the VM to the saved baseline snapshot and ran:
ip a
The address was reported as 10.0.0.2/24 after restoration. This checks that the saved VM state retained the expected interface configuration. For a fuller recovery test, also repeat the route, gateway, internet, and DNS checks.
🎥 Project Walkthrough
A short screen recording of the Kali VM setup and verification:

Watch the project video
🐞 Troubleshooting Notes
Internet connectivity after manual IPv4 configuration
After manually configuring IPv4 settings, internet connectivity failed. One command tried during troubleshooting was:
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
Important technical note: ipv4.dad-timeout relates to duplicate-address detection timing; changing it is not a general fix for missing internet connectivity. Do not rely on this command as the standard solution. Instead, inspect the active connection profile and routing/DNS state:
nmcli connection show --active
nmcli device status
ip address
ip route
cat /etc/resolv.conf
Confirm that the IP address is valid for the NAT Network, the default route is correct, and DNS is configured as intended. If you change a connection profile, reconnect the correct profile and retest.
💡 What I Practiced
1. NAT vs. NAT Network
- NAT: commonly provides outbound connectivity for an individual VM through VirtualBox's network translation.
- NAT Network: allows VMs attached to the same NAT Network to communicate with one another and generally provides outbound connectivity through NAT.
This makes a NAT Network useful when building a multi-machine practice lab. It should not be described as complete isolation from the internet or from every other network path; review the adapter settings and avoid port forwarding unless it is deliberately needed.
2. Virtual machine networking
I configured a VirtualBox virtual network adapter and learned how network mode and guest network settings affect communication.
3. IPv4 configuration
I practiced checking IPv4 addresses, subnet masks, gateways, DNS settings, and connectivity in Kali Linux.
4. VM snapshots
I created a clean baseline snapshot to support recovery after experimental changes.
5. Troubleshooting and documentation
I documented commands, screenshots, configuration details, and issues encountered during setup. Clear technical documentation makes a lab easier to reproduce and maintain.
🔐 Security and Ethical Use
This laboratory is intended for educational purposes and authorized security testing only.
- Test only systems you own or have explicit permission to assess.
- Use intentionally vulnerable targets in a controlled lab.
- Keep vulnerable target VMs off untrusted networks.
- Review shared-folder permissions and avoid exposing sensitive host files.
- Take snapshots before significant changes or risky experiments.
🔗 Tools and Official Resources
- 7-Zip Downloads
- Oracle VirtualBox Downloads
- Kali Linux — Get Kali
- Nmap Official Website
👤 Author
Jitesh Rakheja
Cybersecurity Learner | BCA Student
- LinkedIn: Jitesh Rakheja
📌 Project Information
- Program: Cybersecurity at NetworkWalks
- Week: 01
- Project: Cybersecurity & Penetration-Testing Lab Setup
- Repository: GitHub
