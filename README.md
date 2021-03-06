# NTUST Edge AI (110-02) 期末專題報告_M11007320_黃旭輝

## 1. 作品名稱：【智慧生活】─ 停車場空車位偵測

---

## 2. 摘要說明
透過照片或影片，辨識出停車場中哪裡還有空位，以利於進來停車的消費者能夠更快速的找到停車位，減少因找停車位所浪費的時間。為了要辨識出那裡有車位，本系統選用**物件偵測模型**來實現辨識停車位是否為空的功能

---

## 3. 系統簡介
![](https://i.imgur.com/78PDOqZ.png)

### 3.1 創作發想
在日常生活中，不管是要逛街或是買晚餐，常常因找車位浪費了許多寶貴的時間，如果能夠提前知道哪裡有車位，就能減少找車位的時間。而在目前的停車場中大部分都是使用硬體感測器去判別是否是空車位，雖然這樣的準確率不低，但每個車位都要裝一個感測器，成本非常的高，因此希望透過攝影機經由物件辨識的技術判別哪裡有空車位，提早告訴停車者哪裡有空位，不僅減少裝設感測器的成本，也進而減少找車位的時間，改善生活品質。

### 3.2 硬體架構
本系統可以輸入照片或是影片做為模型的輸入，使用 Google Colab 進行停車場空位模型訓練，並於 Google Colab 上進行照片、影片推論。

|        |           筆電規格        |
| ------ |:------------------------:|
| OS     |      Windows 10 Pro      |
| CPU    | Intel i7-7700HQ 2.80GHz  |
| GPU    | NVIDIA GeForce MX150 2GB |
| RAM    |           12GB           |
| Python |          3.7.13           |

### 3.3 工作原理及流程
使用相機/空拍機所拍攝的影像當作輸入，經過模型推論之後，可以得知停車場的停車狀況

![](https://i.imgur.com/3YuMluc.png)

### 3.4 資料集建立方式
本次可使用的資料集為[PKLot](https://www.kaggle.com/blanderbuss/parking-lot-dataset)
- **PKLot**包含了12,416張，大小為640*640從監控攝影機拍攝的停車場畫面，在資料集中有晴天、雨天、陰天的圖像，停車位有標記為已佔用或空位。


<img src="https://i.imgur.com/Zek698k.jpg" width="320" height="320">  <img src="https://i.imgur.com/x7Yvoic.jpg" width="320" height="320">  <img src="https://i.imgur.com/S6vIwyM.jpg" width="320" height="320">


- 資料集中，occupied及empty車位數量
![](https://i.imgur.com/LIJfsaO.png)
- 資料集中，訓練與測試資料的occupied及empty車位數量
![](https://i.imgur.com/rSUc3iT.png)

### 3.5 模型選用與訓練
本系統的物件偵測模型使用**yolov4-tiny**

![](https://i.imgur.com/oL34eWD.jpg)

在訓練的部分，將原本的yolov4-tiny-custom.cfg做以下的更動：
- class = 2
- batch size = 64
- max_batches = 4000
- steps= 3200 , 3600
- filters = 21

訓練資料分配比例如下：
- train: 8691/12416 (70%)
- valid: 2483/12416 (20%)
- test:1242/12416 (10%)

訓練時間為2小時03分鐘
![](https://i.imgur.com/0JaCNJM.png)


---

## 4. 實驗結果
將拍攝到的相片輸入至模型後，模型將輸出包含bounding box的輸入圖片，用於偵測圖片上那些是停車空位，哪些是有人停的。

**紫紅色bounding box：space-empty**

**綠色bounding box：space-occupied**

### 4.1 成果展示

這三張測試圖片為**資料集中的測試資料**，可以明顯看出模型都能夠準確的標示出空車位及占用車位

<img src="https://i.imgur.com/getowgE.jpg" width="320" height="320"> <img src="https://i.imgur.com/DC4WP5D.jpg" width="320" height="320">

<img src="https://i.imgur.com/u2NkqVG.jpg" width="320" height="320"> <img src="https://i.imgur.com/spmEsPl.jpg" width="320" height="320">

<img src="https://i.imgur.com/euGG7S4.jpg" width="320" height="320"> <img src="https://i.imgur.com/pKWc7Q7.png" width="320" height="320">

### 4.2 測試與比較

這三張測試圖片為**網路上隨機抓取**，可以明顯看出模型都能夠準確的標示出空車位及占用車位

<img src="https://i.imgur.com/2lxvwjQ.jpg" width="414" height="265"> <img src="https://i.imgur.com/P14xL8l.jpg" width="414" height="265">

<img src="https://i.imgur.com/tG7dWzR.jpg" width="414" height="265"> <img src="https://i.imgur.com/04QT6aa.jpg" width="414" height="265">

<img src="https://i.imgur.com/xPbNsxi.jpg" width="414" height="265"> <img src="https://i.imgur.com/pfNUcNk.jpg" width="414" height="265">

這張照片為測試失敗的圖片，因拍攝角度與訓練資料不同，因此測試效果不好

<img src="https://i.imgur.com/D9QJLtx.png" width="407" height="229"> <img src="https://i.imgur.com/PO236ve.jpg" width="412" height="231"> 

### 4.3 改進與優化
這邊提出未來可以改進的方向：
#### 4.3.1 增加訓練資料的多樣性
在測試的時候，有上網抓取一些停車場的圖片進行推論，不過因拍攝角度與訓練資料不同的關係，效果並不是這麼的顯著，因此可能需要多一點其他角度的訓練資料，讓模型學習到更多不一樣角度的停車場圖。
#### 4.3.2 模型收斂問題
在模型訓練中，可以看到當所有的epochs全部跑完時，Loss還停在3.多，因此可以增加epoch數來使模型能夠在收斂

---

## 5. 結論
本系統可以透過物件偵測模型，辨識出停車場的停車格是空位或是已經有車輛停了。若使用與訓練資料角度相同的停車場圖，效果非常的好，但如果角度不大相同，則辨識的成功率就會變得比較差，後續可以透過增加訓練資料的多樣性來解決

---

## 6. 附錄
- [![](http://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1KCOpWNfUcOFsR5R-xJ1bf_XwXA3I72Je)
- [模型權重及相關資料下載 GitHub](https://github.com/frankhuang1999/EdgeAI_Final.git)
- [Hackmd文檔](https://hackmd.io/@hsh/EdgeAI_Final-Project_M11007320)
- [Dataset Download](https://public.roboflow.com/object-detection/pklot)

---

## 7. 參考資料
- [許哲豪 - 如何以Google Colab及Yolov4tiny (yolov4tiny)來訓練自定義資料集 ─ 以狗臉、貓臉、人臉偵測為例](https://omnixri.blogspot.com/2021/05/google-colabyolov4-tiny.html)
- [Github - AlexeyAB/darknet](https://github.com/AlexeyAB/darknet)
- [PKLot - A robust dataset for parking lot classification](https://www.sciencedirect.com/science/article/pii/S0957417415001086)

