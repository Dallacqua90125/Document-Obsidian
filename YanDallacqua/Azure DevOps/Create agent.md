### Create the agent
````powershell
PS C:\> mkdir agent ; cd agent
PS C:\agent> Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\Downloads\vsts-agent-win-x64-4.251.0.zip", "$PWD")
````

### Configure the agent
````powershell
PS C:\agent> .\config.cmd
````

### Optionally run the agent interactively
#### If you didn't run as a service above:

```powershell
PS C:\agent> .\run.cmd
```
