參考來源
1. https://simoncoenen.com/blog/programming/graphics/DoomEternalStudy.html
2. https://home.gamer.com.tw/artwork.php?sn=5114570

> [!TIP] Forward Render
> - 常見的Render Path。
> - 光的數量和複雜度成線性增長，所以Unity只考慮重要程度最大的4個光源(預設)。
> - 在Vertex shader 或 fragment shader計算光源。
> - 若一個1920*1080解析度的畫面有1000個頂點的模型、n個光源，則逐點光複雜度為1000n

> [!TIP] Deferred Render (延遲渲染)
> - 光照處理延遲到clip space  screen space(2D)再進行。
> -需要G-Buffer儲存每個像素的Position、Normal、Diffuse和其他材質資訊，產生紋理圖。
> - 優勢: 將光源數目與物體數目複雜度完全分開。(Light Culling ：只處理在範圍內的光線)。
> - 劣勢: G-Buffer消耗顯卡內存多、存取耗費頻寬大。
> 
> 優化方式(1): Light Pre-Pass:> - G-Buffer只儲存Z值、Normal。 (縮小G-Buffer數據結構)。
> > - 在Fragment shader利用G-Buffer先計算出Light properties，進行alpha-blend後存入LightBuffer。
> > - 最後結果送給Forward Render輸出。
> > - 優點是每個物體能使用不同效果的Shader進行渲染。
> 
> 優化方式(2): Tile-Based Deferred Render:
> 
> > - 將螢幕分成許多小區塊(tile)，一個個處理後重組。一般Tile 是32*32像素[4]。
> > - 常用於手機遊戲，因為手機內存小、快速，所以《By splitting the render target into tiles just small enough to fit in this memory, and processing those one at a time, we minimise the amount of interaction with the slower main memory。》[5] (英文比較好懂XD)
> > - 透過Depth值剔除掉被遮擋的Tile，減省運算。(只適用不透明渲染) [4]


