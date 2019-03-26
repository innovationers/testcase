# testcase

### 1.撰写测试用例注意事项

#### 1.1 延迟时间问题

1.在每次发送按键后，如果会打开新页面，需要延迟2s-3s再进行下一步的动作，否则可能页面没有打开就发送按键，导致无法响应响应事件。
```yaml
  -
    test_desc: ok键
    test_action: send_key_event  #发送遥控器按键，打开新的播放页面
    data: ok                    #键值
    test_sleep: 2000               #键值当前操作结束后等待时间,新开页面之前的动作需要延时
  -
```
2.打开视频采集，最好放在需要马上进行的关键测试点的位置，避免采集太多无用图片，比如:
```yaml
  -
    test_desc: ok键
    test_action: send_key_event  #发送遥控器按键
    data: ok                    #键值
    test_sleep: 2000               #键值当前操作结束后等待时间
  -
    test_desc: 打开采集器视频流检测  #当前步骤描述
    test_action: start_image_stream  #打开采集器视频流检测开关,必须在图片比对操作前打开
    data: 30000                       #采集器图片采集时间，单位毫秒
    test_sleep: 800
  -
    test_desc: ok键
    test_action: send_key_event  #发送遥控器按键，打开新的播放页面
    data: ok                    #键值
    test_sleep: 2000               #键值当前操作结束后等待时间,新开页面之前的动作需要延时
  -
    test_desc: home键
    test_action: send_key_event  #发送遥控器按键
    data: 3                    #键值
    test_sleep: 3000               #键值当前操作结束后等待时间
    tag: time1
  -
    test_desc: 比较像素
    test_action: pixel_compare   #程序开启像素比较，30s超时
    data:
     locations: #坐标值列表
       -  [140, 147]
     rgbs:  #与坐标对应的rgb值列表
       -  (239, 238, 243)
    tag: time2
  -
```
