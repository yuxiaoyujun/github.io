---
title: 【hexo】解決hexo報錯
date: 2019-06-09 12:20:37
tags: 
  - hexo
categories: 程序员的自我修养
---

```bash
：TypeError: Cannot read properties of null (reading '_id')
```

1. **清理 Hexo 數據庫緩存**： 有時候，Hexo 的數據庫文件會損壞，可以通過刪除 `db.json` 文件來強制 Hexo 重新生成數據庫文件。這個文件通常位於 Hexo 根目錄下。

   ```bash
   rm db.json
   ```

   然後重新生成靜態文件：

   ```bash
   hexo clean
   hexo g
   ```

2. **檢查配置文件**： 檢查 `_config.yml` 文件中的配置是否正確，特別是與數據庫相關的配置。

3. **檢查 Hexo 插件**： 有些插件可能會導致數據庫讀取問題。嘗試禁用最近安裝或更新的插件，然後再次生成靜態文件。

   可以通過注釋掉 `_config.yml` 中的插件配置來禁用插件。例如：

   ```bash
   # plugins:
   # - hexo-plugin-name
   ```

4. **檢查 Hexo 日誌**： 檢查 Hexo 的日誌的輸出，尋找更多有關錯誤的線索。日誌通常會包含更多詳細信息，可以幫助確定是哪個特定項目或插件導致了問題。

5. **升級 Hexo 及其依賴項**： 確保 Hexo 和所有插件都是最新版本。有時候，問題已經在新版本中修復。

   ```bash
   npm update hexo
   npm update
   ```

6. **檢查數據文件**： 如果你有自定義的數據文件（例如 `.md` 文件），請檢查這些文件，確保所有文件都是正確格式的，沒有缺失 `_id` 屬性。

如果以上步驟都無法解決問題，請考慮創建一個新的 Hexo 項目，然後逐步將你的內容和配置文件移動到新項目中，以確保沒有數據損壞。
