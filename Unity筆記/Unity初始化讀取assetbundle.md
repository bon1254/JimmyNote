> [!INFO]
> 原始碼如下


```ad-abstract
title: 原始碼

using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Xml;
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.UI;
using Newtonsoft.Json.Linq;

public class AppStarter : MonoBehaviour
{
    public Text appVersion;
    public UpdateLoadView UpdateLoad;
    public enum ServerType
    {
        FormalServer, Dev01Server, Test01Server, BochangeServer
    }
    public ServerType serverType;

    static JObject BaseObj;

    void Awake()
    {
        // 系统配置
        Screen.sleepTimeout = SleepTimeout.NeverSleep;
        QualitySettings.vSyncCount = 0;
        Application.targetFrameRate = 60;

        string baseUrl = "";
        switch (serverType)
        {
            case ServerType.FormalServer://正式機
                baseUrl = "https://gcsw.089457.tw/twfile/GcInit_gcdh.json";
                appVersion.text = "v0." + Application.version;
                break;
            case ServerType.Dev01Server://測試機 (for QA)
                baseUrl = "https://dev01.089457.tw/twfile/GcInit_gcdh.json";
                appVersion.text = "v1." + Application.version;
                break;
            case ServerType.Test01Server://開發機
                baseUrl = "https://test01.089457.tw/twfile/GcInit_gcdh.json";
                appVersion.text = "v2." + Application.version;
                break;
            case ServerType.BochangeServer://柏全測試
                baseUrl = "http://192.168.1.179:8822/twfile/GcInit_gcdh.json";
                appVersion.text = "v3." + Application.version;
                break;
            default:
                appVersion.text = "v?." + Application.version;
                break;
        }

        StartCoroutine(IniteAPP(baseUrl));
    }

    IEnumerator IniteAPP(string baseUrl)
    {
        // 取得基礎資料
        yield return GetJsonFromURL(baseUrl);
        // 下載資源
        yield return GetAssets();
        print("<強連線初始化>");
        print("<遊戲準備完成>");

        // 導入lobby資源
        yield return BaseManager.LoadAssetBundle("prefabs_lobby_ab");
        yield return BaseManager.LoadAssetBundle("image_lobby_ab");
        
        // 登入畫面
        BaseManager.Ins.OpenUI("prefabs_lobby_ab", "Assets/Raw/Prefabs/Lobby/1.LoginView.prefab");

        // 檢測是否斷網
        for (; ; )
        {
            yield return new WaitForSeconds(2f);
            if (Application.internetReachability == NetworkReachability.NotReachable)
            {
                print("請檢查網路是否已經連線");
            }
        }
    }

    IEnumerator GetJsonFromURL(string baseUrl)
    {
        //print("baseUrl:" + baseUrl);
        UnityWebRequest www = UnityWebRequest.Get(baseUrl);
        yield return www.SendWebRequest();
        if (!string.IsNullOrEmpty(www.error))
        {
            print("BaseObj讀取失敗");
            Application.Quit();
            StopAllCoroutines();
            yield break;
        }
        print(www.downloadHandler.text);
        BaseObj = JObject.Parse(www.downloadHandler.text);
    }
    IEnumerator GetAssets()
    {
        string version = Application.platform == RuntimePlatform.IPhonePlayer ? (string)BaseObj["CIV"] : (string)BaseObj["CAV"];
        //是否需要更新版本
        if (version != Application.version)
        {
            print("版號不正確:" + version);
            StopAllCoroutines();
            yield break;
        }
        //下載系統文件
#if UNITY_STANDALONE_OSX
        string AssetUrl = $"https://{BaseObj["Rip"]}/twres/lp_resource/v{Application.version}/pc";
#elif UNITY_STANDALONE_WIN
        string AssetUrl = $"https://{BaseObj["Rip"]}/twres/lp_resource/v{Application.version}/pc";
#elif UNITY_IPHONE
        string AssetUrl = $"https://{BaseObj["Rip"]}/twres/lp_resource/v{Application.version}/ios";
#elif UNITY_ANDROID
        string AssetUrl = $"https://{BaseObj["Rip"]}/twres/lp_resource/v{Application.version}/android";
#else
        string AssetUrl = $"https://{BaseObj["Rip"]}/twres/lp_resource/v{Application.version}/pc";
#endif
        print("AssetUrl:" + AssetUrl);
        UnityWebRequest wwwXML = UnityWebRequest.Get(AssetUrl + "/BundleVersion.xml");
        yield return wwwXML.SendWebRequest();
        //print(wwwXML.downloadHandler.text);

        if (!string.IsNullOrEmpty(wwwXML.error))
        {
            print("xml讀取失敗");
            Application.Quit();
            StopAllCoroutines();
            yield break;
        }
        XmlDocument xml = new XmlDocument();
        xml.LoadXml(wwwXML.downloadHandler.text);
        XmlNodeList nodeList = xml.SelectSingleNode("xml").ChildNodes;
        List<string> AssetBundlePaths = GetUpdateFile(nodeList);//需要額外下載的檔案路徑 
        //有檔案需要更新?
        if (AssetBundlePaths.Count > 0)
        {
            // 打開下載公告
            UpdateLoad.OpenDownloadUI(long.Parse(xml.DocumentElement.GetAttribute("size")));
            // 等到公告被關閉才繼續
            for(; ; )
            {
                yield return null;
                if (!UpdateLoad.Download.activeSelf) break;
            }
            int Index = 0;
            //實作把檔案下載下來
            foreach (string i in AssetBundlePaths)
            {
                UnityWebRequest wwwAssetBundle = UnityWebRequest.Get(AssetUrl + i);
                print(wwwAssetBundle.url);
                yield return wwwAssetBundle.SendWebRequest();
                if (string.IsNullOrEmpty(wwwAssetBundle.error))
                {
                    print($"下載成功:{i}");
                    byte[] buff = wwwAssetBundle.downloadHandler.data;
                    File.WriteAllBytes(Application.persistentDataPath + "/UpgradeBundles" + i, buff);
                }
                else
                {
                    print($"下載失敗:{wwwAssetBundle.error}");
                }
                Index++;
                UpdateLoad.SetBar(AssetBundlePaths.Count, Index);
            }
        }
        // 下載結束關閉讀條
        BaseManager.Ins.GetUI<UpdateLoadView>().Bar.gameObject.SetActive(false);
    }

    /// <summary>透過Xml文件取得需要更新的檔案清單</summary>
    static List<string> GetUpdateFile(XmlNodeList nodeList)
    {
        print($"去路徑:{Application.persistentDataPath}驗證Xml裡的Md5");
        List<string> AssetBundlePaths = new List<string>();//需要額外下載的檔案路徑
        //有UpgradeBundles這個資料夾嗎
        if (Directory.Exists(Application.persistentDataPath + "/UpgradeBundles"))
        {
            //透過md5檢測檔案是否符合
            foreach (XmlElement i in nodeList)
            {
                string FilePath = Application.persistentDataPath + "/UpgradeBundles" + i.GetAttribute("path");
                // 檔案不存在 || md5不符合
                if (!File.Exists(FilePath) || Md5.GetMD5HashFromFile(FilePath) != i.GetAttribute("md5"))
                {
                    AssetBundlePaths.Add(i.GetAttribute("path"));
                }
            }
        }
        else
        {
            //創建資料夾
            Directory.CreateDirectory(Application.persistentDataPath + "/UpgradeBundles");
            //導入所有檔案的資料
            foreach (XmlElement i in nodeList)
            {
                AssetBundlePaths.Add(i.GetAttribute("path"));
            }
        }
        return AssetBundlePaths;
    }
}
```

