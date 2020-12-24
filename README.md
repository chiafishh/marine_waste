# Project - marine_waste
# 一. 重要檔案解釋
- model 架構: 
cfg/CORE_FE_mish.cfg
- 訓練好的 model: 
backup/CORE_FE_mish1129_127000.weights
- 執行檔:  darknet_video_f.py

# 二. 更改執行檔基本內容
### 1. 必須更改
- 跑此程式的電腦的 IP 、設定 Port
        
        # host='IP' , port=Port
        app.run(host='140.121.199.58', debug=False, port=1111, threaded=True)

### 2. 可以更改
- input: 更改下方程式碼的default value。(預設為朝境公園抽水站上方攝影機之RTSP url。)

        # input: the stream(url) or video(path) to be detected
        parser.add_argument("--input", type=str, default="rtsp://testguy:05076416@111.70.5.94:10154/profile1",
                            help="video source. If empty, uses webcam 0 stream")
- 圖片顯示的 width & height: 

        # 圖片顯示的 width & height
        width = 1024
        height = 768

# 三. 確認串流url(顯示即時偵測結果之影像)
url為: http://(IP):(Port)/Stream

(IP)-執行程式之電腦IP 、(Port):設定好的port號 (同前面的設定)

ex. http://140.121.199.58:1111/Stream

# 四. 執行程式
若有"更改"執行檔以外的程式，執行程式前先執行以下兩行指令:

        make clean        
        make -j

## 於網頁上顯示即時偵測結果
### 1.執行指令:
    python darknet_video_f.py 

### 2.觀看結果
- 方式1:打開已經嵌入正確串流url的網頁
    (嵌入範例: ```<img src="http://140.121.199.58:1111/Stream">```)    
    可以打開資料夾內的範例網頁: demo.html

- 方式2:直接在瀏覽器開啟url    
    在瀏覽器網址列輸入url即可

- 補充說明-建置串流
    本計畫將偵測結果影像建置為網頁可讀取之串流，使用的建置工具為Flask


# 五. darknet基本功能-test and train
## test
-  圖片

    1.只須test一張圖片:

    ```./darknet detector test cfg/voc.data cfg/CORE_FE_mish.cfg (weight Path) (圖片Path) ```

    ex.
    
    ```./darknet detector test cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish_last.weights testImg/o2.jpg```

    會自動將偵測結果儲存為predictions.jpg

    2.須test多張圖片：

    ```./darknet detector test cfg/voc.data cfg/CORE_FE_mish.cfg (weight Path) ``` 

    程式顯示"Enter Image Path:"後，輸入圖片Path
    
    補充-可一次輸入多張圖片Path，程式會依序顯示偵測結果
    
    ex.在"Enter Image Path:"處貼上
    
        /home/chiafishh/10_marine/marine_waste/testImg/4352.jpg
        /home/chiafishh/10_marine/marine_waste/testImg/4616.jpg
        /home/chiafishh/10_marine/marine_waste/testImg/4733.jpg

    看完一張，在圖片視窗按"Esc"，程式會自動show下一張
        
- 影片

    ```./darknet detector demo cfg/voc.data cfg/CORE_FE_mish.cfg (weight Path) (影片Path)```

    ex.
    
    ```./darknet detector demo cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish1125_9000.weights  testVideo/1.mp4``` 
    
    ex. (串流影片)
    
    ```./darknet detector demo cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish_last1028.weights  rtsp://testguy:05076416@111.70.5.94:10154/profile1```

    若要儲存結果，在後方加上"-out_filename (欲儲存影片Path)"
   
    ex.
    
    ```./darknet detector demo cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish1125_9000.weights  testVideo/1.mp4 -out_filename testVideo/1_output.mp4```


## train
### 1. 若有新增train or validate data，須重算anchor
1.執行-

   ```./darknet detector calc_anchors cfg/voc.data -num_of_clusters 9 -width 512 -height 512```

2.更新model(cfg檔)的anchor值
   cfg檔有三處有anchor值，三處都要更新

### 2. 確認model(cfg檔)為training模式
在檔案的頂端

### 3. train

- 從頭train

    ```./darknet detector train cfg/voc.data cfg/CORE_FE_mish.cfg CORE_FE_mish.137```

- 接續訓練過的weight檔train

    ```./darknet detector train cfg/voc.data cfg/CORE_FE_mish.cfg (weight Path)```

    ex.

    ```./darknet detector train cfg/voc.data cfg/CORE_FE_mish.cfg backup/filename.weights```
- 假如訓練中斷，可利用last檔繼續訓練
   
    ```./darknet detector train cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish_last.weights```

- 若要在chart(程式跑下去就會顯示的即時訓練chart圖)上顯示 mAP，在指令尾巴加上: ``` -map```

- 查看weight結果(可測多個weight檔案，比較哪一個最好)
        ex.

    ```./darknet detector map cfg/voc.data cfg/yolov4-tiny.cfg backup/CORE_FE_mish_last.weights```

