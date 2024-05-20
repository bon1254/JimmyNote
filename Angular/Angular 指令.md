`npm install -g @angular/cli` 安裝angular:
例如npm install -g @angular/cli@16

`ng version` 確認版本
`ng new client` 生成一個基本的 Angular 應用程式的骨架。
`ng serve` 是 Angular CLI 提供的命令，用於啟動 Angular 應用程式的開發伺服器。

當你運行指令時，Angular CLI 會在預設的埠（通常是 `http://localhost:4200`）上啟動一個開發伺服器，並監聽你的 Angular 專案中的更改。一旦你對專案中的任何文件進行了修改，開發伺服器就會重新編譯應用程式並自動重新載入網頁，讓你能夠即時查看你所做的更改。

1. **ng g c nav --dry-run**
    
    - `ng`: Angular CLI 命令的縮寫。
    - `g`: generate（生成）的縮寫。
    - `c`: component（組件）的縮寫。
    - `nav`: 組件的名稱。
    - `--dry-run`: 試運行模式，這個選項會顯示會生成的文件和更新，但不會實際做出任何更改。
    
    執行這個命令後，CLI 告訴你如果沒有 `--dry-run` 選項，它會創建以下文件：
    
    - `src/app/nav/nav.component.html`: 組件的模板文件。
    - `src/app/nav/nav.component.spec.ts`: 組件的單元測試文件。
    - `src/app/nav/nav.component.ts`: 組件的 TypeScript 邏輯文件。
    - `src/app/nav/nav.component.css`: 組件的樣式文件。
    - 它還會更新 `src/app/app.module.ts` 文件以包含新生成的組件。
2. **ng g c nav --skip-tests --dry-run**
    
    - `--skip-tests`: 命令選項，用於跳過生成測試文件。
    
    這次的命令試運行告訴你如果實際執行命令，它會創建：
    
    - `src/app/nav/nav.component.html`
    - `src/app/nav/nav.component.ts`
    - `src/app/nav/nav.component.css`
    - 並更新 `src/app/app.module.ts` 文件，但不會生成測試文件 `nav.component.spec.ts`。
3. **ng g c nav --skip-tests**
    
    - 這個命令真正執行了生成新組件的操作，因為沒有 `--dry-run` 選項。
    - 創建了 `nav` 組件，包括：
        - `src/app/nav/nav.component.html`
        - `src/app/nav/nav.component.ts`
        - `src/app/nav/nav.component.css`
        - 並更新了 `src/app/app.module.ts` 文件，不包含測試文件。

總結：

- `--dry-run` 選項用來試運行命令以查看會有哪些變更而不實際執行。
- `--skip-tests` 選項用來跳過測試文件的生成。
- 最後一個命令真正生成了組件文件，並且更新了 `app.module.ts` 文件來包含新生成的 `nav` 組件。