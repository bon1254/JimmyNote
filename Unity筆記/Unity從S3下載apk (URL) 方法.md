> [!TIP] 版本資訊
> >版本: 2021.3.15f1
> >平台: Android 

```ad-abstract
title:原始碼

using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using UnityEngine;
using UnityEngine.UI;
using TMPro;

public class DownloadAPK : MonoBehaviour
{
	public TMP_Text Text_Debug;
	public TMP_Text Text_PackageName;
	public TMP_Text Text_Authority;

	void Start()
	{
		StartCoroutine(downLoadFromServer());
	}

	IEnumerator downLoadFromServer()
	{
		string url = "https://s3.ap-northeast-1.amazonaws.com/game-staging.o2metaspace.com/1.apk";

		string savePath = Path.Combine(Application.persistentDataPath, "data");
		savePath = Path.Combine(savePath, "AntiOvr.apk");

		Dictionary<string, string> header = new Dictionary<string, string>();
		string userAgent = "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/55.0.2883.87 Safari/537.36";
		header.Add("User-Agent", userAgent);
		WWW www = new WWW(url, null, header);


		while (!www.isDone)
		{
			//Must yield below/wait for a frame
			Text_Debug.text = "Stat: " + www.progress;
			yield return null;
		}

		byte[] yourBytes = www.bytes;

		Text_Debug.text = "Done downloading. Size: " + yourBytes.Length;


		//Create Directory if it does not exist
		if (!Directory.Exists(Path.GetDirectoryName(savePath)))
		{
			Directory.CreateDirectory(Path.GetDirectoryName(savePath));
			Text_Debug.text = "Created Dir";
		}

		try
		{
			//Now Save it
			System.IO.File.WriteAllBytes(savePath, yourBytes);
			Debug.Log("Saved Data to: " + savePath.Replace("/", "\\"));
			Text_Debug.text = "Saved Data";
		}
		catch (Exception e)
		{
			Debug.LogWarning("Failed To Save Data to: " + savePath.Replace("/", "\\"));
			Debug.LogWarning("Error: " + e.Message);
			Text_Debug.text = "Error Saving Data";
		}

		//Install APK
		installApp(savePath);
	}

	//For API 24 and above
	private bool installApp(string apkPath)
	{
		bool success = true;
		Text_Debug.text = "Installing App";

		try
		{
			//Get Activity then Context
			AndroidJavaClass unityPlayer = new AndroidJavaClass("com.unity3d.player.UnityPlayer");
			AndroidJavaObject currentActivity = unityPlayer.GetStatic<AndroidJavaObject>("currentActivity");
			AndroidJavaObject unityContext = currentActivity.Call<AndroidJavaObject>("getApplicationContext");

			//Get the package Name
			string packageName = unityContext.Call<string>("getPackageName");
			string authority = packageName + ".fileProvider";

			Text_PackageName.text = packageName;
			Text_Authority.text = authority;

			AndroidJavaClass intentObj = new AndroidJavaClass("android.content.Intent");
			string ACTION_VIEW = intentObj.GetStatic<string>("ACTION_VIEW");
			AndroidJavaObject intent = new AndroidJavaObject("android.content.Intent", ACTION_VIEW);


			int FLAG_ACTIVITY_NEW_TASK = intentObj.GetStatic<int>("FLAG_ACTIVITY_NEW_TASK");
			int FLAG_GRANT_READ_URI_PERMISSION = intentObj.GetStatic<int>("FLAG_GRANT_READ_URI_PERMISSION");

			//File fileObj = new File(String pathname);
			AndroidJavaObject fileObj = new AndroidJavaObject("java.io.File", apkPath);
			//FileProvider object that will be used to call it static function
			AndroidJavaClass fileProvider = new AndroidJavaClass("androidx.core.content.FileProvider");
			//getUriForFile(Context context, String authority, File file)
			AndroidJavaObject uri = fileProvider.CallStatic<AndroidJavaObject>("getUriForFile", unityContext, authority, fileObj);

			intent.Call<AndroidJavaObject>("setDataAndType", uri, "application/vnd.android.package-archive");
			intent.Call<AndroidJavaObject>("addFlags", FLAG_ACTIVITY_NEW_TASK);
			intent.Call<AndroidJavaObject>("addFlags", FLAG_GRANT_READ_URI_PERMISSION);
			currentActivity.Call("startActivity", intent);

			Text_Debug.text = "Success";
		}
		catch (System.Exception e)
		{
			Text_Debug.text = "Error: " + e.Message;
			success = false;
		}

		return success;
	}
}

```

> [!INFO] 需要注意的點
> >1.如果JavaClass原本是"android.support.v4.content.FileProvider"，要改成"androidx.core.content.FileProvider"，如下圖
> >![[Androidx.core.content.FileProvider.png]]
> 
> >2.匯出至AndroidStudio做AndroidManifiest做設置
> >3.到gradle.properties中新增android.enableJetifier=true (表示Android插件會通過重寫其二進製文件來自動遷移現有的第三方庫，以使用AndroidX依賴項；未設置時默認為false)和android.useAndroidX=true (表示“Android插件會使用對應的AndroidX庫，而非Support庫)，還有android.enableR8=false記得刪掉。
> >![[gradle.properties (Path).png]]
> >![[gradle.properties.png]]
> 
>> 4.新增以下這段Provider到application底下，並且修正下方錯誤去新增androidx.core (觸摸錯誤地方以顯示)
>>  ![[新增androidx.core.png]]
>
>
>>5.新增xml/provider_paths，透過下方錯誤去新增 (觸摸錯誤地方以顯示)
>>![[新增xml provider_paths.png]]
>
>>6.點選後按下OK即新增 (會新增一個xml資料夾在unityLibrary/res/xml，provider_paths.xml在xml底下)
>>![[新增provider_path.xml畫面.png]]
>>![[xml provider_paths路徑.png]]
>
>> 7.authorities要修改成自己的(packageName).fileProiver
>> ![[authorities要修改成自己的packageName.fileProiver.png]]
>
>>8.uses-feature這要新增安裝許可權通知android.permission.REQUEST_INSTALL_PACKAGES
>>![[uses-feature安裝許可權通知android.permission.REQUEST_INSTALL_PACKAGES.png]]
>
>>9.修改provider_paths內的文件改為底下的
>>![[provider_path的內容.png]]
>
>>10.可以輸出apk了



