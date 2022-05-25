+++
authors = [ "dale" ]
categories = [ "Advanced" ]
description = "This guide covers the creation and deployment of plugin toolbars."
keywords = [ "RhinoCommon", "C/C++", "Rhino", "Toolbar", "Plugin" ]
languages = [ "C#", "C/C++" ]
sdk = [ "RhinoCommon", "C/C++" ]
title = "Creating and Deploying Plugin Toolbars"
type = "guides"
weight = 1

[admin]
TODO = ""
origin = ""
picky_sisters = ""
state = ""

[included_in]
platforms = [ "Windows" ]
since = 0

[page_options]
byline = true
toc = true
toc_type = "single"

+++

 
## Question

How can I create one or more toolbars for my plugin, and how can I deploy these toolbars with my plugin?

## Answer

If you want to create Rhino-style toolbars, then use Rhino's `Toolbar` command. You can save your custom toolbars in your own Rhino User Interface (RUI) file. For details on creating toolbars, see the Rhino help file.

If you give your custom RUI file the exact same name as the plugin RHP file and install it in the folder containing the RHP file, then Rhino will automatically stage it to a writable location and open it the first time your plugin loads.

## More Information

The first time a plugin is loaded, Rhino looks for an RUI file with the same name as the plugin. If it is found, it is copied to the following location and opened:

```
%APPDATA%\McNeel\Rhinoceros\<version>\UI\Plug-ins\
```

It is copied, or staged, to ensure that the file is writable and to provide a way to revert to the original, or default, RUI file if needed.

You can revert to the original, or default, RUI file by deleting the RUI file in the *%APPDATA%* folder and then and restarting Rhino, which will cause the file to be staged again, as the file no longer exists.

Note, there is additional code in Rhino that saves the name of RUI files closed by the user. If a user closes an RUI and the RUI file is associated with a plugin, the file name goes on a list so that Rhino does not automatically open the RUI file in the future. The logic is if the user closed the file, we don't want to keep loading it every time Rhino starts.

Also note, if you uninstall your plugin and manually close the RUI file, within Rhino, you are telling Rhino you no longer want to auto-load the RUI file. Thus, the RUI file will not load if you re-install your plugin. If you were to uninstall your plugin and delete the RUI file from the *%APPDATA%* folder, then the RUI file will load if you re-install your plugin.

Finally, if you update your plugin, Rhino will not re-stage the RUI file because it already exists. You can get Rhino to re-stage the RUI file by deleting it in *%APPDATA%* and restarting which will cause Rhino to copy the file again since it no longer exists. This can be done programmatically by adding the following code to your plugin object's `OnLoad` override.

<div class="codetab">
  <button class="tablinks" onclick="openCodeTab(event, 'cs')" id="defaultOpen">C#</button>
  <button class="tablinks" onclick="openCodeTab(event, 'cpp')">C++</button>
</div>

<div class="tab-content">
<div class="codetab-content" id="cs">

```cs
/// <summary>
/// Called when the plugin is being loaded.
/// </summary>
protected override LoadReturnCode OnLoad(ref string errorMessage)
{
  // Get the version number of our plugin, that was last used, from our settings file.
  var plugin_version = Settings.GetString("PlugInVersion", null);

  if (!string.IsNullOrEmpty(plugin_version))
  {
    // If the version number of the plugin that was last used does not match the
    // version number of this plugin, proceed.
    if (0 != string.Compare(Version, plugin_version, StringComparison.OrdinalIgnoreCase))
    {
      // Build a path to the user's staged RUI file.
      var sb = new StringBuilder();
      sb.Append(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData));
      sb.Append(@"\McNeel\Rhinoceros\6.0\UI\Plug-ins\");
      sb.AppendFormat("{0}.rui", Assembly.GetName().Name);

      // Verify the RUI file exists.
      var path = sb.ToString();
      if (File.Exists(path))
      {
        try
        {
          // Delete the RUI file.
          File.Delete(path);
        }
        catch
        {
          // ignored
        }
      }

      // Save the version number of this plugin to our settings file.
      Settings.SetString("PlugInVersion", Version);
    }
  }

  // After successfully loading the plugin, if Rhino detects a plugin RUI
  // file, it will automatically stage it, if it doesn't already exist.

  return LoadReturnCode.Success;
}

```

</div>

<div class="codetab-content" id="cpp">

```cpp
void CTestlugIn::LoadProfile(LPCTSTR lpszSection, CRhinoProfileContext& pc)
{
  // Get the version number of our plugin that was last used
  pc.LoadProfileString(lpszSection, L"PlugInVersion", m_last_version);
}

void CTestPlugIn::SaveProfile(LPCTSTR lpszSection, CRhinoProfileContext& pc)
{
  // Get the version number of our plugin
  pc.SaveProfileString(lpszSection, L"PlugInVersion", m_plugin_version);
}

BOOL CTestPlugIn::OnLoadPlugIn()
{
  // If the version number of the plugin that was last used does not match the
  // version number of this plugin, proceed.
  if (!m_last_version.IsEmpty() && 0 != m_last_version.CompareNoCase(m_plugin_version))
  {
    // Build a path to the user's staged RUI file.

    ON_wString path;
    CRhinoFileUtilities::GetRhinoRoamingProfileDataFolder(path);
    path += L"UI\\Plug-ins\\";

    ON_wString plugin;
    GetPlugInFileName(plugin);

    ON_wString fname;
    ON_wString::SplitPath(plugin, nullptr, nullptr, &fname, nullptr);
    fname += L".rui";

    path += fname;

    // Verify the RUI file exists.
    if (CRhinoFileUtilities::FileExists(path))
      // Delete the RUI file.
      CRhinoFileUtilities::DeleteFile(path);
  }

  return TRUE;
}
```

</div>
</div>
