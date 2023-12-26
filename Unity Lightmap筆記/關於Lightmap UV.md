1.只知道目前沒有勾選Lightmap UVs很容易有模型上有奇怪陰影和貼圖，勾選之後如果面數太高會很容易造成卡頓問題，所以要盡量減少模型以及面數。


關於Lightmap 烘培時間
1.如果先烘培好了的話，不要整個大模型進入，不然烘培時間會拉很長。

2.為什麼會有烘培完黑點的問題
有勾產生lightmapUV的前提下，下面參考網址
https://www.reddit.com/r/Unity3D/comments/11k4sa4/dark_faces_and_random_light_spots_in_baked/

This random lights spots is a very common lightmapper issue. It happen when there is a very bright geometry in the baking area. This can be a light bulb that has intensive white emissive surface. Important is that the emissive area is very small. This causes artifacts because lightmappers cast rays in random directions and some rays will hit a lot brighter area that will contribute a lot of light to calculated pixel. Try to remove any emission intensive geometry from your scene.

This can be also causes by using hdr skybox where the sun is much more brighter, so try to also switch the skybox to one without the sun.

Dark spots are issues that comes often from too little texel density and some lightmappers try to fix it by using some clever algorithm. If increasing lightmap texel density does not help, i will suggest to use different lightmapper, like Bakery, because unity built-in lightmapper cant handle this case nicely.

這種隨機光點是一個非常常見的光照貼圖問題。當烘焙區域中有非常明亮的幾何形狀時，就會發生這種情況。這可以是具有密集的白色發射表面的燈泡。重要的是發射面積非常小。這會導致偽像，因為光照貼圖器沿著隨機方向投射光線，並且某些光線會擊中更亮的區域，這將為計算的像素貢獻大量光線。嘗試從場景中刪除任何發射密集型幾何體。

這也可能是由於使用陽光較亮的 hdr 天空盒造成的，因此請嘗試將天空盒切換到沒有陽光的天空盒。

黑點通常是由於紋素密度太小而產生的問題，一些光照貼圖器嘗試使用一些巧妙的演算法來解決它。如果增加光照貼圖紋素密度沒有幫助，我會建議使用不同的光照貼圖器，例如 Bakery，因為 Unity 內建的光照貼圖器無法很好地處理這種情況。

3.第三種解決方式

網路上找到的方法是調低Scale in lightmap，目前從1調低到0.8
參考網址:https://sunra.top/2022/03/05/unity-bake-ground-black/

具體的解決方案，是修改地形的Mesh Renderer -> LightMapping -> Scale in lightmap參數，調低此參數。

此參數的意思是設定物件的UV在光照貼圖中的單位數量。

一個物體最終產生的光照貼圖的大小，由該參數，Windows->Lighting->Lightmap Resolution一起決定，可以理解為二者的乘積，然後由於Window->Lighting->Max Lightmap Size的限制，超過該大小 ，就會再拆出一個光照貼圖。

Scale in lightmap越大，該物體在光照貼圖中的細節越多，而對於地形這種東西，其實沒必要很多細節，而且由於太大了，如果細節很多，最終的貼圖會很大，最後，會 產生黑斑。

當然，引起黑斑還有其他一些原因，但我暫時沒有遇到，