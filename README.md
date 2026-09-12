# 🏫 Branch Administration Automation Suite (Joy English)
> 專為連鎖英語分校打造的行政自動化排班與交接表生成系統，整合 A3 Word 班表解析引擎與 B5 雙月輪值動態順延產生器。

[![Java](https://img.shields.io/badge/Java-17-orange.svg)]()
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2.5-brightgreen.svg)]()
[![Apache POI](https://img.shields.io/badge/Apache%20POI-5.2.5-blue.svg)]()
[![Architecture](https://img.shields.io/badge/Architecture-Layered%20MVC-lightgrey.svg)]()

---

## 📌 專案背景與解決痛點

分校行政主管每週與每雙月需花費數小時手動編排助教交接表與菁英班輪值表。傳統作業面臨兩大核心瓶頸：
1. **排版耗時且易錯**：手動將 Excel 班表轉錄為 Word 時，常因寒暑假時段異動（14:00 起始 vs 16:00 起始）及休假人員替換，導致版面溢出或打叉劃記錯位。
2. **輪值順延連動繁瑣**：雙月輪值遇到國定假日或臨時調班時，需逐日手動重新推算後續同仁輪替順序。

本系統透過 **Spring Boot MVC** 搭配 **Apache POI 底層 XML 操作**，實現上傳 Excel 瞬間原生生成符合印刷規格之 A3 Word 交接表，並提供前端可視化 B5 雙月自動順延排印機制。

---

## 📸 成果展示 (Demo)

### 1. 系統功能導航中心
<!-- 請在此放主選單 index.html 截圖 -->
![Main Menu](demo/main_menu.png)

### 2. A3 助教交接表自動化生成系統
<!-- 請在此放交接表操作流程 GIF 或生成的 Word 預覽截圖 -->
| 操作介面 (支援學期/寒暑假模式切換) | 原生產出之 A3 Word 交接表 (排版精確控制) |
| :---: | :---: |
| ![Handover UI](demo/handover_ui.png) | ![A3 Word Result](demo/handover_result.png) |

* **特色**：精準計算儲存格寬度（DXA），底層注入 OpenXML 結構實現跨格對角劃記（Diagonal Borders）與 100% 單頁自動高度防爆限制。

### 3. B5 雙月菁英班輪值表產生器
<!-- 請在此放右鍵點擊放假、名額自動順延的 GIF 動畫 -->
![Roster Demo](demo/roster_demo.gif)

* **特色**：右鍵即時切換「放假 / 值班」，演算法自動動態重算後續週期的循環指針（Pointer-shifting）；CSS Print 精確鎖定 B5 Portrait 單頁。

---

## 🛠 技術棧與架構設計

### 後端架構 (Layered MVC)
* **Core Framework**: Spring Boot 3.2.5 (Java 17)
* **Document Engine**: Apache POI 5.2.5 (`poi-ooxml`, `poi-ooxml-full`)
* **Data Persistence**: Jackson JSON (`fixed_tasks.json`, `roster_config.json`)
* **Web Server**: Embedded Tomcat (Spring Web Starter)

```text
com.handover.handover_web
├── HandoverWebApplication.java   // 容器啟動進入點
├── HandoverController.java       // 交接表 RESTful 路由與檔案串流
├── HandoverService.java          // Excel 動態解析與 Word OOXML 排版核心
├── RosterController.java         // 輪值設定持久化 API
└── RosterService.java            // 輪值配置 JSON 讀寫
