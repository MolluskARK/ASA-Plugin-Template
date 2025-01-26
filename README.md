# ASA Plugin Template
Simple template and guide for building **ARK: Survival Ascended** server plugins using [AsaApi](https://github.com/ArkServerApi/AsaApi).
## Requirements
- A copy of this repository, including the AsaApi submodule found in `extern\AsaApi\`
- [Visual Studio 2022](https://visualstudio.microsoft.com/vs/community/)
  - The "Desktop development with C++" workload should be installed via the Visual Studio Installer
  - Visual Studio must be configured to use vcpkg (launch the Visual Studio Developer Command Prompt under Tools -> Command Line, and enter `vcpkg integrate install`)
- An **ARK: Survival Ascended** dedicated server with AsaApi and the latest [x64 MSVC Redistributable](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170) installed

**Note:** This plugin template is up-to-date with AsaApi v1.18. If you require a newer version of the AsaApi headers, you should pull the latest code from [AsaApi](https://github.com/ArkServerApi/AsaApi) into the `extern\AsaApi\` submodule.
## 1: Obtain AsaApi.lib
To use AsaApi, plugins must link against the AsaApi library.
### Option 1: Build AsaApi Yourself
Open `extern\AsaApi\AsaApi.sln` with Visual Studio, set the build configuration to `Release` and platform to `x64`, and build the solution. Once the build has completed, `AsaApi.lib` can be found in `extern\AsaApi\out_lib\`. The plugin template project is configured to find the library under that path.

**Note:** The AsaApi project is configured to build with MSVC toolset v14.39.33519. This can be installed under the "Individual components" tab in the Visual Studio Installer: `MSVC v143 - VS 2022 C++ x64/x86 build tools (v14.39-17.9)(Out of Support)`.
### Option 2: Download The Released AsaApi Build
Download the AsaApi release from [ark-server-api.com](https://ark-server-api.com/resources/asa-server-api.31/) or [GitHub](https://github.com/ArkServerApi/AsaApi/releases). Unzip the file and copy `AsaApi.lib` from the `Lib\` directory to `extern\AsaApi\out_lib\` (you may have to create this directory).

To avoid compatibility issues, you should download the release version that matches the code in the `extern\AsaApi\` submodule.
## 2: Modify The Plugin Template
Open `PluginTemplate.sln` with Visual Studio and rename the solution and project with your desired plugin name. The plugin file produced by the build will be named `[ProjectName].dll`.

Add your plugin code to `src\Main.cpp`, or add additional source code files.
#### void Plugin_Init()
This function is called by AsaApi when your plugin is loaded. This is a good place to do plugin setup such as initializing logging and installing hooks.
#### void Plugin_Unload()
This function is called by AsaApi when your plugin is unloaded. Be sure to uninstall any hooks that your plugin has set and perform any other necessary cleanup.
## 3: Modify PluginInfo.json
`PluginInfo.json`, found in `configs\`, holds information about your plugin that is used by AsaApi when the plugin is loaded.
#### FullName
The name of your plugin that AsaApi will display on startup.
#### Description
A description of your plugin that AsaApi will display on startup.
#### Version
The version of your plugin. Be sure to increase this value when releasing a new plugin build.
#### MinApiVersion
The minimum version of AsaApi required to load your plugin. I recommend setting this to the version of AsaApi that you have built your plugin with or the lowest AsaApi version you have tested with.
#### Dependencies
An array of plugin names that are required by your plugin. After loading all plugins on startup, AsaApi will check if any plugins have unmet dependencies and display warnings.
## 4: Install Your Plugin
After building your plugin solution, the output can be found in the `out\` directory.
#### [PluginName].dll
This is your plugin. Your plugin can be installed on an **ARK: Survival Ascended** server by placing this DLL, along with `PluginInfo.json`, into the server's `ShooterGame\Binaries\Win64\ArkApi\Plugins\[PluginName]\` directory.
#### [PluginName].dll.arkapi
This is simply a copy of your plugin DLL that can be used to ease testing. If your plugin is already loaded, you can drop `[PluginName].dll.arkapi` into the `ShooterGame\Binaries\Win64\ArkApi\Plugins\[PluginName]\` server directory and AsaApi will automatically reload the plugin and rename this file to `[PluginName].dll`.

**Note:** AsaApi's automatic plugin reloading can be configured in AsaApi's `config.json` file on the server.
#### [PluginName].pdb
This file contains debug info that provides more detailed information when using a debugger or if the server is producing crash logs. If you want to take advantage of this file, you can place it in the server's `ShooterGame\Binaries\Win64\ArkApi\Plugins\[PluginName]\` and AsaApi will automatically move it into `ShooterGame\Binaries\Win64\`.
