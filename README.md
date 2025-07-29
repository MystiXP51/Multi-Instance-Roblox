# Multi-Instance-Roblox

Hi guys, I'm posting a small guide here for anyone who wants to play with multiple accounts (more than 2).

# Remind : For double account

For dual accounts, there's an easy solution: use one account through the Roblox Player (via the Roblox website — the browser version), and the second account through the Roblox app from the Microsoft Store. This works very well without any interface conflicts.

# Mutli account ( 3 and more )

But once you try to use 3 or more accounts, things get trickier. I used to use Roblox Account Manager, but since a recent update, it’s become more complicated.

However, there’s still a way to create separate game instances for each account (Dark Doodad posted this on the CAP Discord server).

I'm sharing it here too, for those who are wondering.

You’ll need the Roblox app from the Microsoft Store (updated), and PowerShell.
<img width="863" height="275" alt="Design sans titre (2)" src="https://github.com/user-attachments/assets/f9d9b05f-0a85-4eaf-afdf-a5139e0114b6" />


You’ll also need to have Developer Mode enabled in your PC settings. 
<img width="695" height="337" alt="Capture d'écran 2025-07-29 190050" src="https://github.com/user-attachments/assets/777df2c2-d44b-4f2b-83a6-12424110bf13" />


# Open PowerShell, and you'll see this interface.
<img width="1100" height="611" alt="Capture d'écran 2025-07-29 190925" src="https://github.com/user-attachments/assets/d20d541d-10c8-413a-9448-b881952b25c7" />


Copy and paste this script, then press Enter (you’ll get a message asking for confirmation — accept it) :

```
$Username = Read-Host -Prompt "Please enter the Roblox username"

$Identity = "ROBLOXCORPORATION.ROBLOX.$Username"
$DestPath = "C:\Roblox\$Username"

if (Get-AppxPackage -Name $Identity) {

    Write-Host "UWP instance for $Username already exists."
    return
}

if (Test-Path -Path $DestPath) {

    Remove-Item -Path $DestPath `
                -Recurse        `
                -Force
}

$App = Get-AppxPackage -Name "ROBLOXCORPORATION.ROBLOX"

Copy-Item -Path $App.InstallLocation `
          -Destination $DestPath     `
          -Recurse

Remove-Item -Path "$DestPath\AppxSignature.p7x"

[xml]$Xml = Get-Content -Path "$DestPath\AppxManifest.xml"

$Xml.Package.Identity.Name = "$Identity"
$Xml.Package.Properties.DisplayName = "$Username"
$Xml.Package.Applications.Application.VisualElements.DisplayName = "$Username"
$Xml.Package.Applications.Application.VisualElements.DefaultTile.ShortName = "$Username"
$Xml.Save("$DestPath\AppxManifest.xml")

Add-AppxPackage -Path "$DestPath\AppxManifest.xml" -Register
```

<img width="1115" height="628" alt="Capture d'écran 2025-07-29 190953" src="https://github.com/user-attachments/assets/31da5e6e-6a98-4882-8e52-e90bd6bb4537" />


It will then ask you to give a name to the new instance (to help you remember, you can use the account’s username, for example — avoid special characters like "_", "!", etc. The "-" might be allowed, but keep in mind it's just for naming the instance, which creates a game window for each Roblox account).


So, enter the name and press Enter when you have this step (I write "MystiXP" for exemple)
<img width="828" height="136" alt="Design sans titre (5)" src="https://github.com/user-attachments/assets/41bf2d63-0efa-487c-8fb7-f1a2268bd4a3" />



Once the process starts, you'll reach this step. Press "Enter" to Register the new interface and start creating it.
<img width="1115" height="628" alt="Capture d'écran 2025-07-29 191311" src="https://github.com/user-attachments/assets/57046c60-d601-4ff9-86e8-b951c57f7b15" />


Once it’s done, you should see the window like this (waiting for a new request). You can copy the script again and run it to create another instance.
<img width="1115" height="628" alt="Capture d'écran 2025-07-29 191445" src="https://github.com/user-attachments/assets/ddc9ae12-4ac6-4159-b401-61548fc24583" />



Once the interface is created, search for the name on your computer and you’ll see the Roblox icon (which you can pin to your taskbar).
Log in normally with your username and password for each account.

<img width="867" height="462" alt="Design sans titre (6)" src="https://github.com/user-attachments/assets/680ee04b-3c4f-4acf-8cfb-8a8576d64184" />


# Tips

To avoid disconnection bugs in each session, make sure to log out properly through the in-game menu before closing the window.






