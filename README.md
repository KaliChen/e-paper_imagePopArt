# 用e-paper做普普風格影像顯示

這次使用的元件是1.54inch_e-paper_b (黑白紅顯示)

參考網址如下：

[https://github.com/soonuse/epd-library-python/tree/master/1.54inch_e-paper_b](https://github.com/soonuse/epd-library-python/tree/master/1.54inch_e-paper_b)

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
在顯示的時候黑色電子墨水會先顯示， 接下來紅色電子墨水以暈染的型態浮現在黑色電子墨水上層．

