# ha_daily_english
HA里面每日一句的实体，API可以提供中英文句子互译，图片，和英文发音音频。

卡片效果如下
![20231010160824](https://github.com/WalterDSU/ha_daily_english/assets/91654066/79c25484-e629-4ef4-b29f-5da9ee6572d2)

在configuration.yaml添加下面senor，来提供每日一句的内容获取, 需要重新HA，并确定sensor.daily_english正常显示。
```
sensor：
  - platform: rest
    name: daily_english
    resource: http://open.iciba.com/dsapi
    value_template: >-
          {% if value_json.dateline == states("sensor.date")  -%}
            有效
          {% else %}
            无效
          {% endif %}
    scan_interval: 10800
    json_attributes:
      - content
      - note
      - picture
      - tts

```
卡片代码：
```
type: markdown
content: |
  ### {{ state_attr("sensor.daily_english", "content")}}
  译文： {{ state_attr("sensor.daily_english", "note")}}
    ![aaa](  {{ state_attr("sensor.daily_english", "picture")}})

  <audio src="{{ state_attr("sensor.daily_english", "tts")}}" controls></audio>
title: 每日一句
```



