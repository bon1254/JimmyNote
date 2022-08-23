# GooglePlay Activity、服務或廣播接收器沒有明確聲明值

輸出bundle到GooglePlay的問題。

錯誤原因如下：

**警告：**如果 Activity、服務或廣播接收器使用 Intent 過濾器並且沒有明確聲明的值`**android:exported**`，則您的應用無法安裝在運行 Android 12 或更高版本的設備上。

解法：在AndroidManifest的activity上加入android:exported = “true”

![[/GooglePlay Activity、服務或廣播接收器沒有明確聲明值 截圖]]