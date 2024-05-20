`aria-label` 是一個用於提高無障礙性的屬性，屬於 WAI-ARIA（Web 無障礙應用程式）規範的一部分。這個屬性用來為元素提供額外的文本標籤，特別是在元素沒有可視化標籤的情況下。`aria-label` 常用於改善屏幕閱讀器用戶的體驗，使他們能夠更好地理解頁面內容和功能。

### `aria-label` 屬性的應用

以下是一些常見的使用情況和範例：

#### 1. 按鈕

當按鈕沒有文本或只有圖標時，可以使用 `aria-label` 來描述按鈕的功能。

html

複製程式碼

`<button aria-label="關閉">✖️</button>`

#### 2. 表單輸入欄位

當輸入欄位沒有可視化標籤時，可以使用 `aria-label` 提供描述。

html

複製程式碼

`<input type="text" aria-label="用戶名" placeholder="請輸入您的用戶名">`

#### 3. 圖標連結

當連結僅由圖標組成時，可以使用 `aria-label` 來描述連結的目的。

html

複製程式碼

`<a href="https://www.example.com" aria-label="首頁">   <img src="home-icon.png" alt=""> </a>`

### `aria-label` 的作用

- **無障礙性**：`aria-label` 可以幫助使用屏幕閱讀器的用戶更好地理解網頁內容和功能。
- **隱藏標籤**：與可視化標籤不同，`aria-label` 的文本不會顯示在頁面上，只會被屏幕閱讀器讀取。

### 與其他屬性的區別

- **`aria-labelledby`**：這個屬性用來引用頁面上其他元素的ID，這些元素的文本內容會被用作當前元素的標籤。適用於需要複用已有標籤的情況。
    
    html
    
    複製程式碼
    
    `<label id="name-label">姓名</label> <input type="text" id="name" aria-labelledby="name-label">`
    
- **`aria-describedby`**：這個屬性用來引用提供描述性文本的元素的ID，這些描述性文本可以幫助用戶理解元素的用途或狀態。
    
    html
    
    複製程式碼
    
    `<input type="text" id="name" aria-describedby="name-desc"> <small id="name-desc">請輸入您的全名</small>`
    

總結來說，`aria-label` 是一個強大且靈活的屬性，能顯著提升網頁的無障礙性，幫助所有用戶更好地互動和理解網頁內容。