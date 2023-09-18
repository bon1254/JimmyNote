> [!TIP] 版本資訊
> >版本: 2021.3.15f1
> >平台: Android 
> 

```ad-abstract
title: 原始碼

void LogFormat(string condition, string stackTrace, LogType type)
		{
			var time = DateTime.UtcNow.ToString("yyyy-MM-dd_HH-mm-ss");

			var msg = $"time -> {time}, type -> {type}, condition -> {condition}, stackTrace -> {stackTrace}";

			using (StreamWriter writer = new StreamWriter(logPath, false))
			{
				writer.WriteLine(msg);
			}
		}
	
```
