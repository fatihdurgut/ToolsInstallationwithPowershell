# Development tools installation with Powershell

```powershell
    # Function to install Chocolatey (package manager)
    function Install-Chocolatey {
        Set-ExecutionPolicy Bypass -Scope Process -Force;
        [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor  [System.Net.SecurityProtocolType]::Tls12;
        iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'));
    }

    # Install Chocolatey if not already installed
    if (-Not (Get-Command choco -ErrorAction SilentlyContinue)) {
        Write-Output "Installing Chocolatey...";
        Install-Chocolatey;
    } else {
        Write-Output "Chocolatey is already installed.";
    }

    # Install Python
    choco install -y python

    # Install Visual Studio Code
    choco install -y vscode

    # Install NodeJS (includes npm)
    choco install -y nodejs

    # Install .NET 9
    choco install -y dotnet-sdk

    # Install Windows Terminal
    choco install -y microsoft-windows-terminal

    # Install Git
    choco install -y git

    # Wait for VSCode to install before installing extensions
    Start-Sleep -Seconds 10

    # Script for batch installing Visual Studio Code extensions
    # Specify extensions to be checked & installed by modifying $extensions
    $extensions = @(
        "ms-python.python",
        "ms-dotnettools.csharp"
    )

    $cmd = "code --list-extensions"
    Invoke-Expression $cmd -OutVariable output | Out-Null
    $installed = $output -split "\s"

    foreach ($ext in $extensions) {
        if ($installed.Contains($ext)) {
            Write-Host $ext "already installed." -ForegroundColor Gray
        } else {
            Write-Host "Installing" $ext "..." -ForegroundColor White
            code --install-extension $ext
        }
    }

    # Start a new PowerShell window to run npm install command
    $npmScript = {
        npm install -g http-server
        Write-Output "http-server installed successfully.";
    }
    Start-Process powershell -ArgumentList "-NoExit", "-Command", $npmScript
    
    Write-Output "All prerequisites, npm, and extensions have been installed successfully."; 

```
