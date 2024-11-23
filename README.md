# FinalProject

## 簡介

這是 FinalProject 的專案，用於展示 Vue 和 Flask 的前後端整合，並實現資料的交互。

## 專案結構

final/
├── backend/                  # Flask 後端
│   ├── app.py                # 主應用程式檔案
│   ├── environment.yml       # Conda 環境配置文件
│   ├── requirements.txt      # 可選，若需要 Python 的 Pip 依賴
│   └── ...
├── frontend/                 # Vue 前端
│   ├── public/               # 靜態檔案
│   ├── src/                  # Vue 源碼
│   └── ...
└── README.md                 # 專案說明文件

## 開始使用

這些是設置和運行專案的步驟。

### 1. 創建並激活 Conda 環境

你需要安裝 Conda。可以使用以下命令創建並激活 Conda 環境：

```bash
cd backend/
conda env create -f [environment.yml](http://_vscodecontentref_/4)
conda activate flask
```

### 2. 啟動 Flask 伺服器

在後端資料夾內，啟動 Flask 伺服器：

```bash
cd backend/
python app.py
```

Flask 伺服器會在 <http://127.0.0.1:5000> 運行。

### 4. 安裝並運行 Vue 前端

你需要安裝 Node.js 和 npm，然後在前端資料夾中運行：

```bash
cd frontend/
npm install
npm run serve
```

Vue 前端會在 <http://localhost:8080> 運行。

## 前後端整合

前端（Vue）會通過 <http://127.0.0.1:5000/api/data> 跟後端（Flask）進行資料交互。
Flask 預設會提供 API，並允許 CORS（跨源資源共享），這樣 Vue 前端就能夠從不同端口的伺服器請求資料。
技術栈

### 前端

Vue 3
Vue CLI
axios 用於 HTTP 請求

### 後端

Flask
Flask-CORS 用於處理跨域請求
pandas、numpy 用於資料處理

## 開發者設置

1. 克隆專案
首先，你需要將專案克隆到本地：

```bash
git clone https://github.com/XiaoWang1212/data_visual.git
cd final
```

1. 安裝後端環境
在後端資料夾內創建並激活 Conda 環境：

```bash
cd backend/
conda env create -f environment.yml
conda activate flask
```

1. 安裝前端依賴
安裝 Node.js 和 npm，並在前端資料夾安裝依賴：

```bash
cd frontend/
npm install
```

常見問題

1. 啟動 Flask 伺服器時出現錯誤
請確保你的 Conda 環境已經成功激活，並且沒有其他應用佔用了 5000 端口。你可以在 app.py 中更改端口配置
