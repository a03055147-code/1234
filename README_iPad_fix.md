
# What's in Your Hand? — iPad 方案2修正版

這包檔案已套用 **方案2（等比縮放 + `--vh` 變數 + `visualViewport`）**，避免 iPad Safari 的 `100vh`/工具列造成內容被裁切。

## 使用方式
1. 將整包上傳到 Github Pages、Netlify 或任何靜態伺服器。
2. 入口頁面：`index.html`。
3. 拍貼流程：`stage3 → booth-preview.html → booth-duo-ipad.html → booth-export.html`。  
   - `booth-export.html` 為我新增的簡易輸出頁（避免 404），會顯示兩張拍攝結果，正式版可再改為合圖/下載。

## 重要調整摘要
- **CSS**
  - `.fit` 使用：
    ```css
    transform: scale(calc(min(100vw/基準寬, (var(--vh, 1vh)*100)/基準高)));
    transform-origin: center center;
    ```
  - `.outer` 具備 `height: 100vh;` + `@supports (height: 100dvh)` 與 `height: calc(var(--vh, 1vh) * 100)` 的備援 class。

- **JS**
  - 全頁皆加入：
    ```js
    (function(){
      function setVH(){
        var h = (window.visualViewport ? window.visualViewport.height : window.innerHeight);
        document.documentElement.style.setProperty('--vh', (h * 0.01) + 'px');
      }
      setVH();
      window.addEventListener('resize', setVH);
      window.addEventListener('orientationchange', setVH);
      document.addEventListener('DOMContentLoaded', () => {
        document.querySelectorAll('.outer').forEach(n => n.classList.add('use-vh-var'));
      });
    })();
    ```

## 注意
- 這些頁面引用了素材檔（例如：`心理測驗模板.jpg`, `動作1.png`, `動作2.png`, `icon.png`）。請將對應圖片放在同一層資料夾。 
- `booth-duo-ipad.html` 使用相機預覽，iOS 需 **HTTPS 網域** 或 **加到主畫面** 才能使用。

祝 demo 順利！
