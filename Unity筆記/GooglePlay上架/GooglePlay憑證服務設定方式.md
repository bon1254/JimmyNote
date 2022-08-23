# GooglePlay憑證服務設定方式

OAuth 2.0 用戶端 ID 設置

![[SHA-1.png]]

名稱就自己打，套件名稱也是。

SHA-1 指紋憑證設置如下：

1.切換到Java的Bin資料夾，打上指令cd C:\Program Files (x86)\Java\jre1.8.0_291\bin 這是我的，橘色字部分路徑打上自己的路徑。

2.查詢指令keytool -list -v -keystore “E:\github\AR ShootBoxes\user.keystore”，橘色字部分Keystore路徑要打上自己的。

3.輸入好之後打上當初設置Keystore的密碼。

4.複製好SHA-1上你憑證指紋就可以了。

![[SHA-2.png]]