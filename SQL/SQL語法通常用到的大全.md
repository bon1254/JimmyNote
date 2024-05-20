-- 從資料表中選擇特定欄位的資料。
SELECT Id, Name
FROM Categories;

-- 向資料表中插入新的資料列。
-- 允許插入指定的識別值
SET IDENTITY_INSERT Categories ON;

-- 插入指定的識別值
INSERT INTO Categories (Id, Name)
VALUES (1, '帥哥');

-- 禁止插入指定的識別值（建議）
SET IDENTITY_INSERT Categories OFF;

-- 更新的方式
UPDATE Categories
SET DisplayOrder = '5', Name = '帥哥'
WHERE Id = '6';

-- 建立資料表
CREATE TABLE NewTable (
    Id int PRIMARY KEY IDENTITY(1,1),
    Name varchar(255)
);

-- 刪除資料表內的資料行
DELETE FROM NewTable WHERE Id = '資料列內的id';

-- 刪除指定資料表
DROP TABLE NewTable;

-- 修改現有的資料表結構
ALTER TABLE Categories ALTER COLUMN Name VARCHAR(255);

-- ORDER BY: 對結果按照指定的列進行排序。
-- ASC 表示升序（由小到大），而 DESC 表示降序（由大到小）。

-- 按照指定列進行升序排序
SELECT * FROM table_name ORDER BY column1 ASC;

-- 按照指定列進行降序排序
SELECT * FROM table_name ORDER BY column1 DESC;
