
各平台導向位置

Windows Store Apps: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) points to `C:\Users\<user>\AppData\LocalLow\<company name>`.  

Windows Editor and Standalone Player: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) usually points to `%userprofile%\AppData\LocalLow\<companyname>\<productname>`. It is resolved by [SHGetKnownFolderPath](https://docs.microsoft.com/en-us/windows/win32/api/shlobj_core/nf-shlobj_core-shgetknownfolderpath) with FOLDERID_LocalAppDataLow, or [SHGetFolderPathW](https://docs.microsoft.com/en-us/windows/win32/api/shlobj_core/nf-shlobj_core-shgetfolderpathw) with CSIDL_LOCAL_APPDATA if the former is not available.  

WebGL: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) points to `/idbfs/<md5 hash of data path>` where the data path is the URL stripped of everything including and after the last '/' before any '?' components.  

Linux: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) points to `$XDG_CONFIG_HOME/unity3d` or `$HOME/.config/unity3d`.  

iOS: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) points to `/var/mobile/Containers/Data/Application/<guid>/Documents`.  

tvOS: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) is not supported and returns an empty string.  

Android: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) points to `/storage/emulated/<userid>/Android/data/<packagename>/files` on most devices (some older phones might point to location on SD card if present), the path is resolved using [android.content.Context.getExternalFilesDir](https://developer.android.com/reference/android/content/Context#getExternalFilesDir(java.lang.String)).  

Mac: [Application.persistentDataPath](https://docs.unity3d.com/ScriptReference/Application-persistentDataPath.html) points to the user Library folder. (This folder is often hidden.) In recent Unity releases user data is written into `~/Library/Application Support/company name/product name`. Older versions of Unity wrote into the `~/Library/Caches` folder, or `~/Library/Application Support/unity.company name.product name`. These folders are all searched for by Unity. The application finds and uses the oldest folder with the required data on your system.
