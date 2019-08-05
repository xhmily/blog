---
title: Jquery实现一键全选/反选/不选
date: 2019-08-05 15:37:32
categories: Jquery
tags: 
- Jquery
- js
top:
password:
---

##### 使用jQuery实现的一键全选/反选/不选

###### 页面代码：

```html
<div id="box">
    <input type="checkbox" value="1">小王
    <input type="checkbox" value="2">小李
    <input type="checkbox" value="3">小张
    <input type="checkbox" value="4">老刘
    <input type="checkbox" value="5">老瓜
    <input type="checkbox" value="6">老毕
</div>
<div>
    <input type="checkbox" id="orChecked">全选/反选/全不选
</div>
```

###### js代码：

```javascript
$('#orChecked').change(function(){
  if($(this).is(':checked')){
     var box = $('#box').children(':checkbox');
      // 复选框长度和没选中的个数一样 -> 全选
     if(box.length==box.filter(':not(:checked)').length){    
      $('#box').children(':checkbox').prop('checked',true);
     }else{
        // 如果有选中个数，-> 反选 
        $('#box').children(':checkbox').each(function(){     
           $(this).prop('checked',$(this).is(':checked')?false:true);
        });
      }
  }else{
      // 如控制键取消选中，剩余的checkbox也取消选中
      $('#box').children(':checkbox').prop('checked',false);    
  }
});
```

