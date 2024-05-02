# ASA Plugin Template
Simple template and guide for building **ARK: Survival Ascended** server plugins using [AsaApi](https://github.com/ArkServerApi/AsaApi).
## Requirements
- A copy of this repository, including the AsaApi submodule found in `extern\AsaApi\`
- [Visual Studio 2022](https://visualstudio.microsoft.com/vs/community/)
  - The "Desktop development with C++" workload should be installed via the Visual Studio Installer
  - Visual Studio must be configured to use vcpkg (launch the Visual Studio Developer Command Prompt under Tools -> Command Line, and enter `vcpkg integrate install`)
- An **ARK: Survival Ascended** dedicated server with AsaApi installed
## 1: Build AsaApi
In order to create fully functional plugins, we must first build the AsaApi library for our plugin to link against. Open `extern\AsaApi\AsaApi.sln` with Visual Studio, set the build configuration to `Release` and platform to `x64`, and build the solution. Once the build has completed, `AsaApi.lib` can be found in `extern\AsaApi\out_lib\` (the plugin template project is configured to find the library under this path).

**Note:** This plugin template is up-to-date with AsaApi v1.15. If you require a newer version of the AsaApi library or headers, you should pull the latest code from [AsaApi](https://github.com/ArkServerApi/AsaApi) into the `extern\AsaApi\` submodule.
## 2: Modify The Plugin Template
Open `PluginTemplate.sln` with Visual Studio and rename the solution and project with your desired plugin name. The plugin file produced by the build will be named `[ProjectName].dll`.

Add your plugin code to `src\Main.cpp`, or add additional source code files.
#### void Plugin_Init()
This function is called by AsaApi when your plugin is loaded. This is a good place to do plugin setup such as initializing logging and installing hooks.
#### void Plugin_Unload()
This function is called by AsaApi when your plugin is unloaded. Be sure to uninstall any hooks that your plugin has set and perform any other necessary cleanup.
#### Logging System
In `src\Main.cpp`, modify this line to use your own plugin name to initialize the logging system:
```c++
Log::Get().Init("PluginTemplate");
```
## 3: Modify PluginInfo.json
`PluginInfo.json`, found in `configs\`, holds information about your plugin that is used by AsaApi when the plugin is loaded.
#### FullName
The name of your plugin that AsaApi will display on startup.
#### Description
A description of your plugin that AsaApi will display on startup.
#### Version
The version of your plugin. Be sure to increase this value when releasing a new plugin build.
#### MinApiVersion
The minimum version of AsaApi required to load your plugin. I recommend setting this to the lowest version of AsaApi that you have tested with.
#### Dependencies
An array of plugin names that are required by your plugin. After loading all plugins on startup, AsaApi will check if any plugins have unmet dependencies and display warnings.
## 4: Build Your Plugin
After building your plugin solution, the output can be found in the `out\` directory.
#### [PluginName].dll
This is your plugin. Your plugin can be installed on an **ARK: Survival Ascended** server by placing this DLL, along with `PluginInfo.json`, into the server's `ShooterGame\Binaries\Win64\ArkApi\Plugins\[PluginName]\` directory.
#### [PluginName].dll.arkapi
This is simply a copy of your plugin DLL that can be used to ease testing. If your plugin is already loaded, you can drop `[PluginName].dll.arkapi` into the `ShooterGame\Binaries\Win64\ArkApi\Plugins\[PluginName]\` server directory and AsaApi will automatically reload the plugin and rename this file to `[PluginName].dll`.

**Note:** AsaApi's automatic plugin reloading can be configured in AsaApi's `config.json` file on the server.
#### [PluginName].pdb
This file contains debug info that provides more detailed information when using a debugger or if the server is producing crash logs. If you want to take advantage of this file, place it in the server's `ShooterGame\Binaries\Win64\` directory.
