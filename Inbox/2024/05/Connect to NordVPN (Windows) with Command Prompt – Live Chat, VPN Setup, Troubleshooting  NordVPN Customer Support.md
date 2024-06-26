---
created: 2024-05-05T10:59:54 (UTC -03:00)
tags: []
source: https://support.nordvpn.com/hc/en-us/articles/19919384880145-Connect-to-NordVPN-Windows-with-Command-Prompt
author: 
---

# Connect to NordVPN (Windows) with Command Prompt – Live Chat, VPN Setup, Troubleshooting | NordVPN Customer Support

> ## Excerpt
> To connect to NordVPN app using the Command prompt, you must first navigate to the directory of NordVPN installation by entering the following command:cd "C:\Program Files\NordVPN\"Change the direc...

---
To connect to NordVPN app using the Command prompt, you must first navigate to the directory of NordVPN installation by entering the following command:

**_cd "C:\\Program Files\\NordVPN\\"_**

_Change the directory if it is not in the default path._

After that, enter any of these commands:

1.  **nordvpn -c**  
    or  
    **nordvpn --connect** - Quick connect to VPN. Use additional arguments to connect to the wanted server:

-   **nordvpn -c -n** **<name>**  
    or  
    **nordvpn -c --server-name** **<name>** - Connect to server by Name.
    
    _Example: **nordvpn -c -n "United States #3710"**_
    
-   **nordvpn -c -g <group>**  
    or  
    **nordvpn** -c **\--group-name <group> -** Connect to best server in specified group (specialty server group name or country).
    
    _Example: **nordvpn -c -g "United States"** (connect to a specific country)  
    or  
    **nordvpn -c -g "Dedicated IP"** (connect to a specific specialty server group)_
    

2.  **nordvpn -d** or **nordvpn --disconnect -** Disconnect from the VPN.
