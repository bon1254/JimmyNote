出現錯誤如下:
1.
Library\PackageCache\com.unity.services.core@1.0.1\Editor\Core\AsyncOperation\AsyncOperationExtensions.cs(39,23): error CS0433: The type 'Task' exists in both 'Unity.Tasks, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null' and 'mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'

2.
Library\PackageCache\com.unity.services.core@1.0.1\Editor\Core\AsyncOperation\AsyncOperationExtensions.cs(107,23): error CS0433: The type 'Task<T>' exists in both 'Unity.Tasks, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null' and 'mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089'

解決辦法:
把GoogleSignInPlugin/Assets的Pares資料夾砍掉

