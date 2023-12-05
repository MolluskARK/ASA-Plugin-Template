# ASA Plugin Template
Simple template for ARK: Survival Ascended server plugins using [GameServerHub's ServerAPI](https://github.com/ServersHub/ServerAPI).


# Requirements
- Visual Studio
- vcpkg

# Cloning with ArkApi
Adding --recursive will pull all required or library repository automatically
```
git clone <your_created_repo_template_link.git> --recursive
```

# VCPKG Installation
- Open Visual Studio Installer
- Click Modify
- Go To Individual components tab
- search for vcpkg package manager and check
- Then Download & Install
- Open Solution
- Go to Tools Tab under Command Line
- Open Developer PowerShell or Command Prompt
- type in
```
vcpkg integrate install
```

