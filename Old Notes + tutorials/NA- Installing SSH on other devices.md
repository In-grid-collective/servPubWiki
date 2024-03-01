### For Mac

Install Homebrew - a package manager for macOs https://brew.sh/

To do so from the terminal use the following command:

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Once installed use the following command to install openssh:

```shell
brew install openssh
```

### For windows

#### Prerequisites check

To validate your environment, open an elevated PowerShell session and do the following:

- Type _winver.exe_ and press enter to see the version details for your Windows device.
    
- Run `$PSVersionTable.PSVersion`. Verify your major version is at least 5, and your minor version at least 1. Learn more about [installing PowerShell on Windows](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows).
    
- Run the command below. The output will show `True` when you're a member of the built-in Administrators group.
    
    ```
    (New-Object Security.Principal.WindowsPrincipal([Security.Principal.WindowsIdentity]::Get
    ```


To install OpenSSH using PowerShell, run PowerShell as an Administrator. To make sure that OpenSSH is available, run the following cmdlet:

```
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

The command should return the following output if neither are already installed:

```
Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```

Then, install the server or client components as needed:

```
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

Both commands should return the following output:

```
Path          :
Online        : True
RestartNeeded : False
```