#marine_waste
###若有更改執行檔以外的程式，執行程式前先執行以下兩行指令:

    make clean
    make -j

#  於網頁上顯示即時偵測結果
## 1.執行指令:
    python darknet_video_ws.py 

## 2.開啟會連接上websocket server的網頁
-可在本地開啟範例網頁(於此資料夾中):demo.html

# test
## 圖片
    ./darknet detector test cfg/voc.data cfg/CORE_FE_mish.cfg weight檔 圖片檔 

ex.

    ./darknet detector test cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish_last.weights testImg/o2.jpg 
    
## 影片
    ./darknet detector demo cfg/voc.data cfg/CORE_FE_mish.cfg weight檔 影片檔
-若要儲存結果，在後方加上"-out_filename 欲儲存檔"

ex.

    ./darknet detector demo cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish1125_9000.weights  testVideo/1.mp4

ex.

    ./darknet detector demo cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish1125_9000.weights  testVideo/1.mp4 -out_filename testVideo/1_output.mp4

ex. (串流影片)

    ./darknet detector demo cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish_last1028.weights  rtsp://testguy:05076416@111.70.5.94:10154/profile1

# train
## 若有新增train or validate data，先重算anchor
1.執行-
./darknet detector calc_anchors cfg/voc.data -num_of_clusters 9 -width 512 -height 512

2.更新model(cfg檔)的anchor值

## 確認model(cfg檔)為training模式(檔案內容開頭幾行)

## train
## #從頭train
    ./darknet detector train cfg/voc.data cfg/CORE_FE_mish.cfg CORE_FE_mish.137

## #接續訓練過的weight檔train
    ./darknet detector train cfg/voc.data cfg/CORE_FE_mish.cfg weight檔

ex.

    ./darknet detector train cfg/voc.data cfg/CORE_FE_mish.cfg backup/filename.weights
-假如訓練中斷，可利用last檔繼續訓練
   
    ./darknet detector train cfg/voc.data cfg/CORE_FE_mish.cfg backup/CORE_FE_mish_last.weights
