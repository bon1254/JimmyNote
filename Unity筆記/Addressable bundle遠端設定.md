參考連結 :https://www.youtube.com/watch?v=KJbNsaj1c1o&t=1s

環境: Google Cloud Storage
1.設定所有為連結全部Remote 然後必須勾選BuildRemote Catelog![[Pasted image 20240311215534.png]]
2.有被勾起來addressable才會被選進去 grounps內
![[Pasted image 20240311215703.png]]
3.點開build -> New Build -> Default build做初次的包檔案。
4.預設remote build會在serverdata資料夾，如果沒有系統會自動建立，然後會包含這些檔案。
![[Pasted image 20240311215912.png]]
5.建立一個google cloud storage然後包含Android資料夾整包放進去
![[Pasted image 20240311220038.png]]
![[Pasted image 20240311220020.png]]
6.Remote Load Path必須設置到資料夾為止，因為必須讀取catelog，例如 ("[https://storage.googleapis.com/testtaiwangoaddressable/Android/]")
![[Pasted image 20240311220202.png]]
7.如果要更新bundle必須update一下，選擇bin檔案 (build->update a previous build)
![[Pasted image 20240311220402.png]]8.一樣會build出來，然後會再設置的remote build path
![[Pasted image 20240311220448.png]]