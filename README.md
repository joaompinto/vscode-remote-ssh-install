# Installing vscode remote ssh server manually

## Motivation

The default behavior of VSCode remote ssh extension when connecting to a remote server is to attempt to download a file from public internet «failing on private networks». While there is an option to fallback to download the file locally and transfer it, such option also fails under certain browser/proxy configurations.

## Requirements

* Windows 10 with VSCode installed

## How to

Open a Windows Powershell terminal window:

```ps
# Set target to the user@hostname you want to connect to.
$target = "user@hostname"

# Get installed vscode commit it
$vscode_version = code -v
$vscode_commit = $vscode_version | Select -Index 1

# Download the linux server component
Invoke-WebRequest -Uri "https://update.code.visualstudio.com/commit:${vscode_commit}/server-linux-x64/stable" -OutFile  "vscode-server.tar.gz"

# Copy and extract it
scp vscode-server.tar.gz ${target}:/tmp/vscode-server.tar.gz
ssh ${target} mkdir -p ~/.vscode-server/bin/${vscode_commit}
ssh ${target} tar zxvf /tmp/vscode-server.tar.gz -C ~/.vscode-server/bin/${vscode_commit} --strip 1

ssh ${target} touch ~/.vscode-server/bin/${vscode_commit}/0

```

> **Warning:**
> You will need to re-run this download everytime you upgrade your vcode as the extension version is also updated.

