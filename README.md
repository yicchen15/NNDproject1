# NND-Project1

### 題目：
請自行用MINST資料集做training data來訓練分類器。

Validation data共有200張本課程同學的手寫圖供測試，每張的尺寸為28X28像素，灰階，uint8格式。

A. 習慣讀取idx3-ubyte圖檔格式的同學請下載validation-images-idx3-ubyte.gz，其
分類標籤請下載validation-labels-idx3-ubyte.gz
B. 習慣讀取jpg圖檔格式的linux愛用者請下載validation.tar; windows愛用者請下載validation.zip，

其分類標籤(label)請下載valdation.csv(喜愛用excel的同學可以下載valdation.xlsx的格式)

Project1有兩部分，第一部分是程式佔80%，第二份是紙本Project佔20%。

程式部分的評分方式，老師會用另外200張手寫數字圖做為測試集，你程式辨識出來的正確率即為程式部分的成績。程式必須可以自動(不可手動按200次)分辨這新的200張圖，程式最後輸出為一個.txt,.csv,或.xlsx，其存有200X1的向量，代表此200圖的分類。

紙本Project:需有簡介、流程、結論、心得、參考來源、程式碼。


-------------------------------------------------
### Project
### 簡介：
NIST全稱NIST Special Database 19，全數據集包含了80多萬張圖片MNIST則是NIST數據集的縮減版，因為NIST數據集儲存的方式集數據分類構成較難使用，而MNIST作為NIST的子，圖片量較少且僅含有數字，更易操作。MNIST 資料集資料包含圖片與標籤兩種形式的內容。其中training data（共 55000 筆）與 validation data（共 5000 筆）皆具有圖片及標籤，而 test data（共 10000 筆）則是只有圖片，沒有標籤。每一筆資料的照片是由 28 pixels x 28 pixels ，總共 784 個 pixels 所組成，圖片顯示的是 0 到 9 之中的一個阿拉伯數字；資料的標籤則表示該手寫數字圖片所呈現的數字為何，也就是 0 到 9 中一個數值。

### 流程：
#### A.	參數設置 
- learning_rate = 5\*1e-4
- num_steps = 1500 (訓練1500次)
- batch_size = 150 (將資料集任取150筆為一組)
- display_step = 100 (每100次顯示一次準確率)
#### B. 測試集圖片影像處理
運用opnecv套件，cv2.threshold將圖片二值化cv2.connectedComponentsWithStats找出圖片內容的位置，進行裁切並用cv2.resize縮放得到24\*24的圖片，cv2.copyMakeBorder再填補空白得到與原先相同大小28\*28的圖片，最後cv2.erode將圖片腐蝕可減少噪點。
#### C. 調整資料型態
MNIST資料集的每張圖片(x)為1\*784的向量，故將200張圖片由28\*28 reshape成1\*784；資料集讀取時已將圖片標籤(y)選用獨熱編碼，故也將csv內容進行獨熱編碼，以方便計算準確率。
#### D. 架構神經網路
線性回歸模型以Wp+b經過Softmax函數得到分類。建構卷積神經網路，選用Tensorflow套件，架構依序輸入層、2個隱藏層(包括卷積層與池化層)，
再經過平滑層、ReLU激活函數，避免Overfitting 故Dropout 一半的特徵，最後輸出層以Softmax函數得到預測結果。運用cross_entropy來得到loss值再修正Weights和biases。
#### E.	訓練神經網路
依照參數與架構放入MNIST訓練集資料進行批次訓練。
#### F.	辨識測試集並輸出csv檔
用訓練完成的神經網路架構對老師提供的測試集進行辨識，並將辨識結果寫入csv檔。

### 結論：
對於MNIST數據集，如用一般神經網路架構訓練後測試MNIST測試集，經過1500次訓練，正確率大約可達86%，雖然增加訓練次數能夠提高準確率，但用卷積神經網路經過1500次訓練正確率可達96%以上。這次project使用MNIST訓練，老師提供圖片做測試，一般線性回歸模型準確率僅有0.115，使用卷積神經網路正確率達0.46，再經圖片前處理，準確率提高到0.645，更進一步對圖片腐蝕準確率則可提高到0.7。一開始需選定模型，並設定參數決定Filter個數與卷積次數，為避免過擬合所以加入dropout提高模型泛化能力。
解決模型後，則需對數據做前處理來提高辨識率，圖片的處理包含二值化、抓取內容裁切、調整大小、腐蝕，這些動作能夠使數據與MNIST資料庫較相近，像是背景值為0、內容值為1，裁切掉多餘的白邊，腐蝕可以減少噪點。


### 參考資料：
###### https://zhuanlan.zhihu.com/p/55045479
###### https://www.tensorflow.org/datasets/catalog/mnist
###### https://ithelp.ithome.com.tw/articles/10197505 卷積網路
###### https://morvanzhou.github.io/tutorials/machine-learning/tensorflow/5-01-classifier/
###### https://fgc.stpi.narl.org.tw/activity/videoDetail/4b1141305d9cd231015d9d0852c5002b (TensorFlow 基礎篇〈中〉)
###### https://fgc.stpi.narl.org.tw/activity/videoDetail/4b1141305d9cd231015d9d08fb62002d (TensorFlow 基礎篇〈下〉)
###### https://ithelp.ithome.com.tw/articles/10188044 ([第 30 天] 深度學習（4）卷積神經網絡與鐵人賽總結)
###### https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/457073/ (Tensorflow深度學習入門——優化訓練MNIST資料和呼叫訓練模型識別圖片) 
###### https://medium.com/jameslearningnote/%E8%B3%87%E6%96%99%E5%88%86%E6%9E%90-%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92-%E7%AC%AC5-1%E8%AC%9B-%E5%8D%B7%E7%A9%8D%E7%A5%9E%E7%B6%93%E7%B6%B2%E7%B5%A1%E4%BB%8B%E7%B4%B9-convolutional-neural-network-4f8249d65d4f ([資料分析&機器學習] 第5.1講: 卷積神經網絡介紹(Convolutional Neural Network))
##### 神經網路 無卷積
###### https://blog.techbridge.cc/2018/01/27/tensorflow-mnist/ (使用 TensorFlow 來做簡單的手寫數字辨識) 
##### 迴歸
###### https://www.itread01.com/content/1542116416.html (Tensorflow之MNIST手寫數字識別：分類問題（2）)
###### https://www.itread01.com/content/1541824521.html (【深度學習】Tensorflow——CNN 卷積神經網路 2)
###### https://arbu00.blogspot.com/2017/05/4-tensorflowlenet5.html
##### 圖像處理
###### https://books.google.com.tw/books?id=4TB3DwAAQBAJ&pg=SA12-PA12&lpg=SA12-PA12&dq=image+%E5%8A%A0%E7%B2%97+opencv&source=bl&ots=8GbqdQig1n&sig=ACfU3U0YSLNNjSUCEVwiAA2SvZccIKG-5Q&hl=zh-TW&sa=X&ved=2ahUKEwjm8Z7R5ezoAhXBfXAKHRZiBxYQ6AEwBHoECAwQLQ#v=onepage&q&f=false
###### https://www.jianshu.com/p/b5c29aeaedc7 (圖像旋轉)
