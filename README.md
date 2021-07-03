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
        (1).`epd.draw_line`
        (2).`epd.draw_rectangle` 
        (3).`epd.draw_circle`
        (4).`epd.draw_filled_rectangle`
        (5).`epd.draw_filled_circle`
        (6).`epd.display_string_at`
        
```
    epd = epd1in54b.EPD()
    epd.init()
    
    # clear the frame buffer
    frame_black = [0xFF] * int((epd.width * epd.height / 8))
    frame_red epd.draw_rectangle= [0xFF] * int((epd.width * epd.height / 8))
    # For simplicity, the arguments are explicit numerical coordinates
    epd.draw_rectangle(frame_black, 10, 60, 50, 110, COLORED)
    epd.draw_line(frame_black, 10, 60, 50, 110, COLORED)
    epd.draw_line(frame_black, 50, 60, 10, 110, COLORED)
    epd.draw_circle(frame_black, 120, 80, 30, COLORED)
    epd.draw_filled_rectangle(frame_red, 10, 130, 50, 180, COLORED)
    epd.draw_filled_rectangle(frame_red, 0, 6, 200, 26, COLORED)
    epd.draw_filled_circle(frame_red, 120, 150, 30, COLORED)

    # write strings to the buffer
    font = ImageFont.truetype('/usr/share/fonts/truetype/freefont/FreeMonoBold.ttf', 18)
    epd.display_string_at(frame_black, 30, 30, "e-Paper Demo", font, COLORED)
    epd.display_string_at(frame_red, 28, 10, "Hello world!", font, UNCOLORED)
    # display the frame
    epd.display_frame(frame_black, frame_red)
    
    
    # display images
    frame_black = epd.get_frame_buffer(Image.open('black.bmp'))
    frame_red = epd.get_frame_buffer(Image.open('red.bmp'))
    epd.display_frame(frame_black, frame_red)
    
    #epd.sleep()
    
    # You can get frame buffer from an image or import the buffer directly:
    epd.display_frame(imagedata.IMAGE_BLACK, imagedata.IMAGE_RED)

```

