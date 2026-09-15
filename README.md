# Refrigerator-Consultant

運用 YOLO 物件偵測與 LINE Bot 開發的智慧食材管家
**Smart Ingredient Recognition and Recipe Recommendation using YOLO and LINE Bot**

透過影像辨識自動偵測使用者提供的食材，並結合食譜資料集進行推薦，讓使用者可以直接透過 LINE Bot 查詢手邊食材適合製作的料理。

---

## Overview

**開發流程：**

`Data Preprocessing` ➔ `YOLO Training` ➔ `Ingredient Detection` ➔ `Recipe Matching` ➔ `LINE Bot Integration` ➔ `System Testing`

| 開發階段                | 核心技術                               | 對應程式碼                                               |
| :----------------- | :------------------------------- | :------------------------------------------------------- |
| **1. 資料前處理**        | 食譜資料清理、格式整理與食材比對                 | [`data_preprocessing.ipynb`](./data_preprocessing.ipynb) |
| **2. 食材辨識**        | 使用 YOLO 建立食材物件偵測模型                | [`yolo_code.ipynb`](./yolo_code.ipynb)                   |
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
