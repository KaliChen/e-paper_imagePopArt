# 用e-paper做普普風格影像顯示

這次使用的元件是[1.54inch_e-paper_b (黑白紅顯示)](https://github.com/soonuse/epd-library-python/tree/master/1.54inch_e-paper_b)

## Pin Layout

![](https://github.com/KaliChen/e-paper_imagePopArt/blob/main/gpiolayout.jpg)

* VCC -> 3.3
* GND -> GND
* DIN -> MOSI
* CLK -> SCLK
* CS -> 24 (Physical, BCM: CE0, 8)
* D/C -> 22 (Physical, BCM: 25)
* RES -> 11 (Physical, BCM: 17)
* BUSY -> 18 (Physical, BCM: 24)

先大概簡單介紹一下該函式庫，不用特別下載安裝依賴庫設定環境，函式庫直接帶入套用就可以使用了

程式運作程序大致為：
#### 1. 呼叫epd物件 
`epd = epd1in54b.EPD()`
#### 2. 初始化epd物件 
`epd.init()`
#### 3. 清空 frame_black 和 flame_red的緩衝器

```
    frame_black = [0xFF] * int((epd.width * epd.height / 8))
    frame_red = [0xFF] * int((epd.width * epd.height / 8))
```

#### 4. 基本功能
(1).畫線 `epd.draw_line`
        
(2).空心方 `epd.draw_rectangle` 
        
(3).空心圓 `epd.draw_circle`
        
(4).實心方 `epd.draw_filled_rectangle`
        
(5).實心圓 `epd.draw_filled_circle`
        
(6).文字 `epd.display_string_at`
        
(7).載入圖片

```
frame_black = epd.get_frame_buffer(Image.open('圖片一'))
frame_red = epd.get_frame_buffer(Image.open('圖片二'))
```
        
(8).顯示圖層 `epd.display_frame(frame_black, frame_red)`
        
## 電子墨水性質

在顯示的時候黑色電子墨水會先顯示並且可以呈現灰階效果， 隨後紅色電子墨水以暈染的型態浮現在黑色電子墨水上層．

接下來就利用以上電子墨水的特性，以OpenCV來設計影像轉換的演算法，參考如下圖：

![https://github.com/KaliChen/e-paper_imagePopArt/blob/main/Display.jpg](https://github.com/KaliChen/e-paper_imagePopArt/blob/main/Display.jpg)

frame_black 作為勾勒圖像輪廓的線條使用，frame_red 作為填滿封閉區域使用

### 說明
frame_red: 圖像經過灰階處理後以THRESH_BINARY二值化，匡列出塗滿的部位，注意區域色彩如果明度太高，THRESH_BINARY的參數沒跟著校正的話，會匡列不出塗滿區域，請參考[色彩原理](http://www.charts.kh.edu.tw/teaching-web/98color/color2-1.htm)

frame_black: 圖像經過canny找出邊緣輪廓後，用dilate膨脹加粗輪廓（膨脹這段可根據圖像需求追加），再用THRESH_BINARY_INY反轉陰陽．



