# UNIX Dunnet Game Emulator (Shell Script)

## 專案簡介 (About The Project)
這是一個基於 UNIX 檔案系統特性的文字冒險遊戲（Text-based Adventure Game）。
專案使用 C Shell (`csh`) 撰寫，完美重現了經典 Emacs 遊戲 "Dunnet" 的核心解謎流程。
最特別之處在於，**本遊戲並非純粹的程式邏輯模擬，而是將整個遊戲地圖實體化為 UNIX 檔案系統 (File System)**。

* **房間是目錄 (Directories)**：東南西北的移動實際上是透過 Symbolic Links 進行 `cd`。
* **物品是檔案 (Files)**：拾取與丟棄物品，底層執行的是 `mv` 指令。
* **上鎖的門是權限 (Permissions)**：使用 `chmod 000` 鎖門，並在玩家取得鑰匙時執行 `chmod 755` 開門。

## 核心功能與技術展示 (Features & Technical Highlights)
1. **Shell Script Game Engine**: 使用 C shell 實作完整的遊戲迴圈 (REPL)、指令解析與狀態控制。
2. **File System Manipulation**: 大量運用 `ls`, `cd`, `mv`, `chmod`, `touch`, 軟連結 `ln -s` 等系統指令來控制遊戲邏輯。
3. **Regex & Stream Editing (SED)**: 透過 `sed` 搭配自訂腳本 (`PA.sedfile`) 過濾與轉換底層指令輸出。例如將生硬的檔名轉譯為閱讀友善的文字，或是隱藏系統錯誤訊息。
4. **Virtual UNIX within UNIX**: 遊戲末期包含一台需要修復的虛擬 VAX 電腦。登入後，腳本內建了「第二層 Shell」，攔截並客製化了 `cd`, `ls`, `pwd`, `cat`, `uncompress` 等指令，透過替換與封裝將真實系統的 `ls -l` 輸出完美偽裝成 1970 年代的古老系統格式。

## 如何遊玩 (How to Play)
1. 確保系統環境支援 C Shell (`csh`) 與相關基本 UNIX 指令。
2. 解壓縮檔案系統作為遊戲地圖：
   ```bash
   tar -xpf filesystem.tar
   ```
3. 執行遊戲腳本：
   ```bash
   ./dunnet1
   ```
4. 遊戲內支援的指令 (Commands)：
   * **移動**：`n`, `s`, `e`, `w`, `ne`, `nw`, `se`, `sw`
   * **探索**：`l` (look), `x [object]` (examine)
   * **物品管理**：`i` (inventory), `get [object]`, `drop [object]`
   * **特殊動作解謎**：`dig`, `put [object] in [target]`, `type`
