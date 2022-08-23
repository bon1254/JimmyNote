# Unity修正錯誤筆記04 (TextMeshPro)

Unity版本：2020.3.0f1

目標平台：PC

最近在使用TMPro的時候，創了新的字會出現以下警告。

![[TMP1.png]]

上網查了一下，大概似乎是說底線符號一定要出現在字體財產上，只要關掉警告就好，

有興趣知道內容可以到以下網址：

[TextMesh Pro — !The character Ellipsis is not available in font asset — Unity Forum](https://www.blogger.com/u/1/blog/post/edit/1169600120833842387/6015017301947165051#)

關掉警告只要到Project Setting→TextMeshPro→Setting那邊打勾Disable warnings。

![[TMP2.png]]