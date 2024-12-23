# 環境創建
## 筆記本設定
實作中請注意環境創建時的筆記本設定，如果沒有切換GPU那麼在後面訓練模型時會跑不動

![14](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/14.png)

![15](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/15.png)

首先克隆模型（加上' ！'表示執行的是終端命令）

    !git clone https://github.com/ultralytics/yolov5
    
![01](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/1.png)

然後把裡面requirements的txt文件環境給pip下來（注意前面要加上%表示命令行命令）

    %cd yolov5
    %pip install -r requirements.txt

因為colab已經幫我們下載好這些環境了，所以看到很多都顯示‘already satisfied’

![02](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/2.png)

接著檢查一下notebook初始化情況

    import torch
    import utils
    display=utils.notebook_init()

![03](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/3.png)

# 推理
先用自帶的yolov5.pt來推理一下

    !python detect.py

其實這裡可以改的參數有很多，具體都在detect.py裡有說明

![04](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/4.png)

可以看到結果已經保存到runs/detect/exp目錄下  

![05](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/5.png)

把圖片打印出來（也可以直接在文件夾裡看）

    display.Image(filename='runs/detect/exp/zidane.jpg', width=600)

![06](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/6.png)

# 驗證
首先下載用來驗證的數據集，這裡下的是coco128

    torch.hub.download_url_to_file('https://ultralytics.com/assets/coco2017val.zip', 'tmp.zip')  # download (780M - 5000 images)
    !unzip -q tmp.zip -d ../datasets && rm tmp.zip  # unzip

接著運行val.py進行驗證（這裡的可選參數也都有寫在val.py裡）

    !python val.py --data coco.yaml

![07](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/7.png)

# 訓練
執行train.py來訓練模型，這裡自己設置了一些參數，比如batchsize設置成64，訓練次數5次，訓練的數據集用的是coco128（必須要之前下載過的並且在data文件夾裡有對應的yaml文件），優化器用的Adam，具體的參數在train.py文件裡也有說明

    !python train.py --batch 64 --epochs 5 --data coco128.yaml --optimizer Adam

![08](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/8.png)

訓練結果保存在這：

![09](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/9.png)

# 最後用自己訓練出的pt文件去推理一次

首先把best.pt文件放到yolov5主文件夾下，不然會報錯找不到該文件

![10](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/10.png)

再運行detect文件，指定weight為剛剛訓練出的best.pt(修改過)

    !python detect.py --weight /content/yolov5/runs/train/exp/weights/best.pt

![11](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/11.png)

結果保存在runs/detect/exp5裡

![13](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/13.png)

最後打印出來看一下(也可以在資料夾裡面看)

    display.Image(filename='/content/yolov5/runs/detect/exp5/zidane.jpg', width=800)

![12](https://github.com/muchen0926/Artificial-Intelligence-Final-Report/blob/main/%E6%9C%9F%E6%9C%ABcode/12.png)
