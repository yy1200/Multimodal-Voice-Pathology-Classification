# 多模態病理嗓音分類競賽
網頁：https://tbrain.trendmicro.com.tw/Competitions/Details/27

## First stage - ACOUSTIC
Reference：https://ketanhdoshi.github.io/Audio-Classification/

Step 1：Audio Pre-processing：定義 Transforms
- open()：從文件中讀取音頻
- rechannel()：轉換為兩個通道
- resample()：標準化採樣率
- pad_trunc()：調整大小至相同長度
- time_shift()：數據增強：時移
- spectro_gram()：梅爾頻譜圖
- spectro_augment()：數據增強：時間和頻率掩蔽
  
Step 2：自定義 Data Loader
- SoundDS
    - 一種自定義 Dataset 對象，它使用所有音頻轉換來預處理音頻文件並一次準備一個數據項
    - 內置的 DataLoader 對象，它使用 Dataset 對象來獲取單個數據項並將它們打包成一批數據

Step 3：使用 Data Loader 準備批量數據
- 將該數據以 80:20 的比例隨機拆分為訓練集和驗證集

Step 4：Create Model
- CNN：四個生成特徵圖的卷積塊，將數據重新整理成需要的格式，以便可以將其輸入到線性分類器層，最終輸出 10 個類別的預測

Step 5：Training
- 定義 optimizer、loss 和 scheduler

Step 6：Inference
- 通過從原始數據中保留驗證數據集來對未見過的數據進行推斷

## Second stage - MEDICAL
以 XGboost 預測分類

## TWO stage
將經過上述兩個階段的預測結果合併後，再以 XGboost 進行預測
