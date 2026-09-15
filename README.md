# Refrigerator-Construction
運用 YOLO 物件偵測與 LINE Bot 開發的智慧食材管家，透過影像辨識技術自動推薦對應食譜。

## Overview
為了解決日常煮飯的煩惱，本專題開發了一款名為「冰箱食材管家」的工具。使用者只需提供照片或是文字，系統便會利用訓練好的模型判斷圖片中所含的食材類別，並結合資料集查詢對應的食譜，最後將結果透過 LINE Bot 顯示給使用者。

### 開發階段核心技術對應檔案
1. **模型訓練與影像辨識**：YOLO 模型建置與推論 ➔ [`yolo_code.ipynb`](./yolo_code.ipynb), [`data.yaml`](./data.yaml), [`best.pt`](./best.pt)
2. **系統後端與通訊串接**：Flask 伺服器與 LINE Messaging API ➔ [`line_bot_code.ipynb`](./line_bot_code.ipynb)
3. **資料前處理與資料庫**：食譜資料清洗、比對與建置 ➔ [`data_preprocessing.ipynb`](./data_preprocessing.ipynb), [`data.csv`](./data.csv)
4. **專題成果與報告**：完整概念提報簡報 ➔ [`Presentation_Slides.pdf`](./Presentation_Slides.pdf)

## 專題簡介
本專題 (Refrigerator Consultant) 旨在幫助使用者解決「冰箱災難 (disaster from refrigerator)」。結合電腦視覺與通訊軟體，打造一個能快速辨識手邊現有食材、並立即提供料理步驟的智慧助理。

## 核心功能與實作設計
系統提供兩種主要互動模式來解決使用者的需求：
* **圖片辨識**：使用者提供一張包含食材的照片，系統透過模型預測後，回傳包含 `title` (食譜名稱)、`ingredients` (所需食材) 與 `directions` (製作步驟) 的詳細食譜。
* **文字輸入**：使用者直接輸入食材關鍵字，系統比對資料庫後同樣回傳完整的食譜資訊。

## 模型訓練與方法
本專題採用 YOLO 架構進行物件偵測，具體的資料準備與訓練規格如下：
* **辨識類別 **：涵蓋 6 種類別，包含 `egg` (雞蛋)、`tomato` (番茄)、`onion` (洋蔥)、`cauliflower` (花椰菜)、`cabbage` (高麗菜) 與 `shrimp` (蝦子)。
* **資料集規模**：每種食材類別約準備 500 筆資料進行模型訓練，確保辨識的準確度與泛化能力。

## 系統實作成果與測試
經過實際測試，系統在 LINE Bot 介面上的運作流程如下：
1. **接收訊息**：系統接收來自使用者的照片或文字訊息。
2. **影像辨識**：若為照片，系統利用訓練好的 YOLO 模型偵測圖片中的物件，例如成功辨識出 `cabbage` (0.69) 與 `tomato` (0.96) 等標籤與信心分數。
3. **結果反饋**：將偵測到的類別透過 LINE Bot 訊息回傳給使用者確認（例如顯示 `['cabbage', 'tomato']`）。
4. **食譜推薦**：將得出的類別結合食譜資料庫進行檢索，隨即發送包含完整食材比例與料理步驟的文字訊息給使用者。

## 使用技術
* **物件偵測模型**：YOLO
* **通訊介面**：LINE Messaging API
* **後端架構**：Python, Flask, ngrok
* **資料處理**：Pandas, OpenCV (`cv2`)

## 專題資訊
* **專題名稱**：冰箱食材管家 (Refrigerator Consultant) - 人工智慧應用實驗
* **專題成員**：黃妤涵、繆語、李佩臻
