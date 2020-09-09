# NND-Project1
 
簡介：
NIST全稱NIST Special Database 19，全數據集包含了80多萬張圖片MNIST則是NIST數據集的縮減版，因為NIST數據集儲存的方式集數據分類構成較難使用，而MNIST作為NIST的子，圖片量較少且僅含有數字，更易操作。MNIST 資料集資料包含圖片與標籤兩種形式的內容。其中training data（共 55000 筆）與 validation data（共 5000 筆）皆具有圖片及標籤，而 test data（共 10000 筆）則是只有圖片，沒有標籤。每一筆資料的照片是由 28 pixels x 28 pixels ，總共 784 個 pixels 所組成，圖片顯示的是 0 到 9 之中的一個阿拉伯數字；資料的標籤則表示該手寫數字圖片所呈現的數字為何，也就是 0 到 9 中一個數值。

流程：
A. 參數設置
learning_rate = 5*1e-4
num_steps = 1500 (訓練1500次)
batch_size = 150 (將資料集任取150筆為一組)
display_step = 100 (每100次顯示一次準確率)
B. 測試集圖片影像處理
運用opnecv套件，cv2.threshold將圖片二值化cv2.connectedComponentsWithStats找出圖片內容的位置，進行裁切並用cv2.resize縮放得到24*24的圖片，cv2.copyMakeBorder再填補空白得到與原先相同大小28*28的圖片，最後cv2.erode將圖片腐蝕可減少噪點。
C. 調整資料型態
MNIST資料集的每張圖片(x)為1*784的向量，故將200張圖片由28*28 reshape成1*784；資料集讀取時已將圖片標籤(y)選用獨熱編碼，故也將csv內容進行獨熱編碼，以方便計算準確率。
D. 架構神經網路
線性回歸模型以Wp+b經過Softmax函數得到分類。建構卷積神經網路，選用Tensorflow套件，架構依序輸入層、2個隱藏層(包括卷積層與池化層)，再經過平滑層、ReLU激活函數，避免Overfitting 故Dropout 一半的特徵，最後輸出層以Softmax函數得到預測結果。運用cross_entropy來得到loss值再修正Weights和biases。
E. 訓練神經網路
依照參數與架構放入MNIST訓練集資料進行批次訓練。
F. 辨識測試集並輸出csv檔
用訓練完成的神經網路架構對老師提供的測試集進行辨識，並將辨識結果寫入csv檔。
