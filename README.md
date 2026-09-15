# Refrigerator-Consultant

運用 YOLO 物件偵測與 LINE Bot 開發的智慧食材管家
**Smart Ingredient Recognition and Recipe Recommendation using YOLO and LINE Bot**

透過影像辨識自動偵測使用者提供的食材，並結合食譜資料集進行推薦，讓使用者可以直接透過 LINE Bot 查詢手邊食材適合製作的料理。

---

## Overview

**開發流程：**

`Data Preprocessing` ➔ `YOLO Training` ➔ `Ingredient Detection` ➔ `Recipe Matching` ➔ `LINE Bot Integration` ➔ `System Testing`

| 開發階段               | 核心技術                             | 對應程式碼                                                    |
| :----------------- | :------------------------------- | :------------------------------------------------------- |
| **1. 資料前處理**       | 食譜資料清理、格式整理與食材比對                 | [`data_preprocessing.ipynb`](./data_preprocessing.ipynb) |
| **2. 食材辨識**        | 使用 YOLO 建立食材物件偵測模型               | [`yolo_code.ipynb`](./yolo_code.ipynb)                   |
| **3. 食譜推薦**        | 根據辨識結果搜尋對應食譜                     | [`data.csv`](./data.csv)                                 |
| **4. LINE Bot 整合** | Flask、LINE Messaging API 與 ngrok | [`line_bot_code.ipynb`](./line_bot_code.ipynb)           |

---

## 專題成果

### [專題簡報](./Presentation_Slides.pdf)

---

## 專題簡介

日常料理時，使用者經常會遇到「冰箱裡有食材，卻不知道可以做什麼」的問題。傳統的食譜搜尋通常需要使用者自行輸入食材名稱，再逐一尋找適合的料理。

因此，本專題希望結合 **Computer Vision 與通訊軟體**，讓使用者可以直接拍攝手邊的食材，由系統自動辨識圖片中的食材，再根據辨識結果搜尋適合的食譜。

本系統以 **LINE Bot** 作為使用者介面，整合影像辨識、食材搜尋與食譜推薦功能，使使用者不需要另外開啟網頁或應用程式，即可取得料理資訊。

---

## 開發目標

* 使用 YOLO 建立食材物件偵測模型
* 辨識圖片中的多種食材
* 建立食材與食譜資料之間的比對機制
* 支援圖片與文字兩種食材輸入方式
* 使用 LINE Messaging API 提供使用者互動介面
* 整合影像辨識、資料搜尋與訊息回傳流程

---

## 系統架構

本專題將影像辨識、後端服務、食譜資料與 LINE Bot 進行整合。

```text
                    User
                      │
               Image / Text
                      │
                      ▼
              ┌──────────────┐
              │   LINE Bot   │
              └──────┬───────┘
                     │
                     ▼
              ┌──────────────┐
              │    Flask     │
              │   Backend    │
              └──────┬───────┘
                     │
             ┌───────┴────────┐
             │                │
             ▼                ▼
      ┌─────────────┐  ┌───────────────┐
      │ YOLO Model  │  │ Recipe Dataset│
      │   Detection │  │    Search     │
      └──────┬──────┘  └───────┬───────┘
             │                 │
             └────────┬────────┘
                      ▼
              Recipe Recommendation
                      │
                      ▼
                 LINE Response
```

---

## 資料集與前處理

本專題的資料主要分為 **食材影像資料**與**食譜資料**兩部分。

### 食材影像資料

使用食材影像資料訓練 YOLO 物件偵測模型，目前包含 6 種食材：

| Class         | 食材  |
| :------------ | :-- |
| `egg`         | 雞蛋  |
| `tomato`      | 番茄  |
| `onion`       | 洋蔥  |
| `cauliflower` | 花椰菜 |
| `cabbage`     | 高麗菜 |
| `shrimp`      | 蝦子  |

每種食材約準備 500 張影像進行模型訓練。

### 食譜資料

使用 Pandas 對食譜資料進行前處理，整理食譜名稱、食材與料理步驟等資訊，並建立後續食材搜尋與推薦所需的資料格式。

主要欄位包含：

* `title` — 食譜名稱
* `ingredients` — 所需食材
* `directions` — 製作步驟

---

## 方法

### 1. Data Preprocessing

首先對食譜資料進行清理與格式整理，將食材資訊轉換成適合搜尋與比對的格式。

透過食材關鍵字與食譜內容進行比對，使模型辨識結果可以進一步轉換為食譜搜尋條件。

相關程式：

[`data_preprocessing.ipynb`](./data_preprocessing.ipynb)

---

### 2. YOLO Object Detection

本專題使用 **YOLO** 進行食材物件偵測。

與單純的圖片分類不同，物件偵測可以同時取得：

* 食材類別
* Bounding Box
* Confidence Score

因此一張照片中可以同時辨識多種食材。

例如：

```text
Input Image
     ↓
YOLO Detection
     ↓
cabbage : 0.69
tomato  : 0.96
     ↓
['cabbage', 'tomato']
```

模型相關檔案：

* [`yolo_code.ipynb`](./yolo_code.ipynb) — YOLO 模型訓練與推論
* [`data.yaml`](./data.yaml) — Dataset 設定
* [`best.pt`](./best.pt) — 訓練完成的模型權重

---

### 3. Recipe Recommendation

取得 YOLO 辨識結果後，系統將食材類別轉換為食譜搜尋條件，並從食譜資料集中搜尋符合的料理。

例如：

```text
Detected Ingredients
        ↓
['cabbage', 'tomato']
        ↓
Recipe Matching
        ↓
Recipe Dataset
        ↓
title
ingredients
directions
```

最後將搜尋結果整理成適合 LINE Bot 顯示的文字內容。

---

### 4. LINE Bot Integration

使用 **Flask** 建立後端服務，並透過 **LINE Messaging API** 接收與回傳使用者訊息。

系統支援兩種主要互動方式：

#### 📷 Image Input

```text
User
 ↓
Upload Image
 ↓
YOLO Detection
 ↓
Ingredient Recognition
 ↓
Recipe Search
 ↓
LINE Response
```

#### 💬 Text Input

```text
User
 ↓
Input Ingredient
 ↓
Keyword Matching
 ↓
Recipe Search
 ↓
LINE Response
```

開發與測試期間使用 **ngrok** 將本機 Flask Server 暴露至外部，使 LINE Webhook 能夠連接本地端服務。

相關程式：

[`line_bot_code.ipynb`](./line_bot_code.ipynb)

---

## 實作成果

系統完成後，可以透過 LINE Bot 直接進行食材辨識與食譜查詢。

### 1. 食材影像辨識

使用者提供包含多種食材的照片後，YOLO 可以偵測圖片中的不同食材。

例如模型成功辨識：

```text
cabbage : 0.69
tomato  : 0.96
```

並將結果整理為：

```text
['cabbage', 'tomato']
```

### 2. 食譜推薦

取得食材辨識結果後，系統進一步搜尋食譜資料，並回傳：

```text
Recipe Title

Ingredients
- ...
- ...

Directions
1. ...
2. ...
3. ...
```

讓使用者可以直接從 LINE Bot 取得完整料理資訊。

---

## 系統操作流程

```text
┌─────────────────────┐
│  使用者傳送圖片/文字  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│    LINE Messaging   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Flask Backend     │
└──────────┬──────────┘
           │
       ┌───┴────┐
       │        │
     Image     Text
       │        │
       ▼        ▼
     YOLO     Keyword
   Detection   Search
       │        │
       └───┬────┘
           ▼
┌─────────────────────┐
│   Recipe Matching   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Recipe Information │
│ Title / Ingredients │
│ / Directions        │
└──────────┬──────────┘
           │
           ▼
       LINE Bot
```

---

## 開發過程與分析

本專題的開發主要分為 **影像辨識、資料處理與系統整合**三個部分。

首先建立食材影像資料集並訓練 YOLO 模型，使系統可以從使用者提供的照片中取得食材類別。

接著進行食譜資料前處理，將食材資訊與食譜內容整理成適合搜尋的格式，建立食材到食譜的推薦流程。

最後使用 Flask 建立後端服務，並串接 LINE Messaging API，使使用者可以透過 LINE 完成圖片辨識、文字搜尋與食譜推薦。

在實際測試過程中，也發現多食材同時出現在圖片中時，辨識結果可能受到**食材遮擋、拍攝角度、圖片背景與食材外觀差異**影響，因此多食材辨識與推薦結果仍有進一步改善的空間。

---

## 未來發展

目前系統主要支援 6 種食材，未來希望從以下方向進行擴充：

* 增加可辨識的食材種類
* 擴充食譜資料集
* 提升多食材同時辨識的準確度
* 根據使用者擁有的多種食材進行更精準的食譜推薦
* 加入食材庫存與保存期限管理
* 加入營養資訊與個人化推薦
* 將模型部署至雲端或邊緣裝置

---

## 使用技術

| 類別               | 技術                 |
| :--------------- | :----------------- |
| Programming      | Python             |
| Object Detection | YOLO               |
| Backend          | Flask              |
| Messaging        | LINE Messaging API |
| Data Processing  | Pandas             |
| Image Processing | OpenCV             |
| Local Server     | ngrok              |

---

## 專案結構

```text
Refrigerator-Construction/
│
├── yolo_code.ipynb
├── line_bot_code.ipynb
├── data_preprocessing.ipynb
│
├── data.yaml
├── best.pt
├── data.csv
│
├── Presentation_Slides.pdf
│
└── README.md
```

### 檔案說明

| 檔案                         | 說明                            |
| :------------------------- | :---------------------------- |
| `yolo_code.ipynb`          | YOLO 模型訓練、驗證與推論               |
| `line_bot_code.ipynb`      | Flask 與 LINE Messaging API 串接 |
| `data_preprocessing.ipynb` | 食譜資料清理與前處理                    |
| `data.yaml`                | YOLO Dataset 設定               |
| `best.pt`                  | 訓練完成的 YOLO 模型權重               |
| `data.csv`                 | 食譜資料                          |
| `Presentation_Slides.pdf`  | 專題成果簡報                        |

---

## 個人貢獻

在本次三人團隊專題中，我主要負責／參與以下工作：

* **模型建置與訓練**：負責 YOLO 食材物件偵測模型的資料準備、訓練與推論測試。
* **資料處理**：使用 Pandas 整理食譜資料，進行資料清理與食材關鍵字比對。
* **系統整合**：協助 Flask Backend、YOLO 模型與 LINE Messaging API 的整合。
* **功能測試**：測試圖片辨識、文字輸入與食譜推薦流程。
* **專案整理**：整理程式碼、模型與資料檔案，建立 GitHub Repository。

---

## 專題資訊

**專題名稱：** 冰箱食材管家（Refrigerator Consultant）

**英文名稱：** Refrigerator Consultant

**課程：** 人工智慧應用實驗(二)

**專題成員：**

* 黃妤涵
* 繆語
* 李佩臻
