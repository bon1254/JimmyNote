紀錄一下

using System;
using System.Xml;
using System.IO;
using System.Text;
using System.Linq;
using System.Security.Cryptography;
using System.Collections.Generic;
using UnityEngine;
using UnityEditor;

public class AssetBundleExport : EditorWindow
{
    //輸出路徑
    private static string OutputRootPath = string.Empty;
    //來源路徑
    private static string RawRootPath = string.Empty;

    private static BuildTarget buildTarget;

    [MenuItem("Tools/资源打包 %#T")]
    static void Init()
    {
        GetWindow(typeof(AssetBundleExport));
    }

    void OnEnable()
    {
        RawRootPath = "Raw";
#if !UNITY_EDITOR
        Debug.LogError(">>> OutputRootPath Unkown!");
#elif UNITY_STANDALONE_OSX
        buildTarget = BuildTarget.StandaloneOSX;
#elif UNITY_STANDALONE_WIN
        buildTarget = BuildTarget.StandaloneWindows;
#elif UNITY_IPHONE
        buildTarget = BuildTarget.iOS;
#elif UNITY_ANDROID
        buildTarget = BuildTarget.Android;
#else
        Debug.LogError(">>> OutputRootPath Unkown!");
#endif
    }

    private void OnGUI()
    {
        GUILayout.BeginScrollView(new Vector2(0, 0), false, true);

        buildTarget = (BuildTarget)EditorGUILayout.EnumPopup("打包版本", buildTarget);
        switch (buildTarget)
        {
            case BuildTarget.StandaloneWindows:
            case BuildTarget.StandaloneWindows64:
            case BuildTarget.StandaloneOSX:
                OutputRootPath = Application.streamingAssetsPath + "/pc";
                break;
            case BuildTarget.iOS:
                OutputRootPath = Application.streamingAssetsPath + "/ios";
                break;
            case BuildTarget.Android:
                OutputRootPath = Application.streamingAssetsPath + "/android";
                break;
            default:
                OutputRootPath = "";
                break;
        }
        RawRootPath = EditorGUILayout.TextField("打包目標資料夾: ", RawRootPath);
        GUILayout.Label($"會打包到：{OutputRootPath}");
        //GUILayout.Label($"dataPath：{Application.dataPath}");
        if (OutputRootPath != "")
        {
            if (GUILayout.Button("開始打包&生成系統文件"))
            {
                //檢測資料夾是否從在不存在就創建
                if (!Directory.Exists(OutputRootPath)) Directory.CreateDirectory(OutputRootPath);
                //取得打包計畫物件
                List<AssetBundleBuild> buildMap = new List<AssetBundleBuild>();
                GetAssetBundleBuilds(buildMap, RawRootPath + "/Image", ".PNG", ".png", ".JPG", ".jpg");//Image
                GetAssetBundleBuilds(buildMap, RawRootPath + "/Prefabs", ".prefab");//Prefab
                GetAssetBundleBuilds(buildMap, RawRootPath + "/Sounds", ".mp3", ".MP3");//Sounds
                //包檔
                BuildPipeline.BuildAssetBundles(OutputRootPath, buildMap.ToArray(), BuildAssetBundleOptions.None, buildTarget);
                //生成系統文件
                CreateXML(OutputRootPath);
                AssetDatabase.Refresh();
            }

            if (GUILayout.Button("重新生成系統文件"))
            {
                CreateXML(OutputRootPath);
                AssetDatabase.Refresh();
            }
        }
        GUILayout.EndScrollView();
    }

    /// <summary> 將資料夾內的子資料夾符合條件的檔案都轉換成AssetBundleBuild裡的資訊 </summary>
    /// <param name="TargetDirectoryPath"> 資料夾位置 </param>
    /// <param name="TargetTypes"> 哪些副檔名會被選中 </param>
    static List<AssetBundleBuild> GetAssetBundleBuilds(List<AssetBundleBuild> buildMap, string TargetDirectoryPath, params string[] TargetTypes)
    {
        string[] Directories = Directory.GetDirectories(Application.dataPath + "/" + TargetDirectoryPath);
        foreach (string i in Directories)
        {
            buildMap.Add(GetAssetBundleBuild(i, TargetTypes));
        }
        return buildMap;
    }
    /// <summary> 將資料夾內符合條件的檔案都轉換成AssetBundleBuild裡的資訊 </summary>
    /// <param name="TargetDirectoryPath"> 資料夾位置 </param>
    /// <param name="TargetTypes"> 哪些副檔名會被選中 </param>
    static AssetBundleBuild GetAssetBundleBuild(string TargetDirectoryPath, params string[] TargetTypes)
    {
        string[] DirectoryPath = TargetDirectoryPath.Split('/', '\\');
        string DirectoryName = DirectoryPath.Last();

        //取得資料夾內所有副檔名正確的檔案路徑
        string[] files = Directory.GetFiles(TargetDirectoryPath, "*.*", SearchOption.AllDirectories).Where(x => GetTypeOK(x)).ToArray();
        List<string> BuFiles = new List<string>();
        foreach(string i in files)
        {
            BuFiles.Add($"Assets{i.Substring(Application.dataPath.Length)}");
        }
        return new AssetBundleBuild() { assetBundleName =  $"{DirectoryPath[DirectoryPath.Length - 2]}_{DirectoryName}_ab", assetNames = BuFiles.ToArray() };
        //====================================================================================================================================================
        bool GetTypeOK(string File)
        {
            foreach (string i in TargetTypes)
            {
                if (File.EndsWith(i)) return true;
            }
            return false;
        }
    }

    /// <summary> 產生系統文件 </summary>
    static void CreateXML(string ExportPath)
    {
        string[] filePaths = Directory.GetFiles(ExportPath, "*", SearchOption.AllDirectories).Where(x => x.EndsWith("_ab")).ToArray();
        XmlDocument xml = new XmlDocument();
        xml.CreateXmlDeclaration("1.0", "UTF-8", "");
        XmlElement root = xml.CreateElement("xml");//創建根節點

        long Size = 0;
        foreach (string i in filePaths)
        {
            Size += new FileInfo(i).Length;
            XmlElement node = xml.CreateElement("node");//創建子節點
            node.SetAttribute("md5", Md5.GetMD5HashFromFile(i));
            node.SetAttribute("path", i.Substring(ExportPath.Length));
            root.AppendChild(node);
        }
        root.SetAttribute("size", Size.ToString());
        root.SetAttribute("date", DateTime.Now.ToString());
        xml.AppendChild(root);
        xml.Save(ExportPath + "/BundleVersion.xml");
    }
}
