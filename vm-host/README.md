# VM Host — Active Directory & Linux Server

Built on VirtualBox, running on a NAT Network for internal lab connectivity.

## What's here

- Windows Server 2025 (Server Core) promoted to an Active Directory domain 
  controller, configured entirely via PowerShell
- Windows 11 Pro client joined to the domain
- A Group Policy Object enforcing a login banner on domain-joined machines
- Ubuntu Server running Nginx as a standalone web server

## Setup

**Domain controller:**
```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Install-ADDSForest -DomainName "lab.local"
```

**Client domain join:**
```powershell
Add-Computer -DomainName "lab.local" -Credential (Get-Credential) -Restart
```

**Login banner GPO:**
```powershell
New-GPO -Name "Login Banner Policy"
Set-GPRegistryValue -Name "Login Banner Policy" -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System" -ValueName "LegalNoticeCaption" -Type String -Value "Lab Network Notice"
Set-GPRegistryValue -Name "Login Banner Policy" -Key "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System" -ValueName "LegalNoticeText" -Type String -Value "Authorized access only. This is a home lab environment for testing purposes."
New-GPLink -Name "Login Banner Policy" -Target "DC=lab,DC=local"
```

**Nginx web server (Ubuntu):**
```bash
sudo apt update
sudo apt install nginx -y
```

## Troubleshooting notes

**Domain join failing with "the request is not supported":** traced this through 
network connectivity, DNS resolution, and Kerberos port reachability (all fine) 
before finding the actual cause — the client was running Windows 11 **Home**, 
which doesn't support domain join at all. Reinstalled with Windows 11 **Pro** 
and the join succeeded immediately once networking was reconfirmed.

**Bridged networking failure:** VirtualBox's bridged adapter mode failed at the 
network layer (ARP-level failures, no DHCP lease) on this host, traced to a 
Windows-side bridging driver issue. Worked around it using a VirtualBox NAT 
Network instead, which resolved connectivity between VMs immediately.

**GPO login banner not displaying despite showing as "Applied":** the GPO's 
`LegalNoticeCaption` registry value kept arriving blank on the client even 
after the DC confirmed it was set correctly, while `LegalNoticeText` always 
replicated fine. Removing and re-adding just that one registry value, then 
forcing a full policy refresh with a reboot, resolved it.

## Screenshots

See `/screenshots` for verification: AD Users and Computers, domain join 
confirmation, GPO application (`gpresult`), the login banner appearing on the 
sign-in screen, and the Nginx welcome page.
