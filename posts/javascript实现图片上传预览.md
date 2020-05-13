---
title: 'javascript实现图片上传预览'
date: 2019-07-07 23:37:20
tags: [js]
published: true
hideInList: false
feature: 
isTop: false
---

```html
<!doctype html>
<html lang="en">
<head>
<!-- Required meta tags -->
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

<title>Hello, world!</title>

<script type="text/javascript">

function viewimg(file){
if(file.files&&file.files[0])//当文件存在
{
var img=document.getElementById("image1");
var reader=new FileReader();//新建文件读写器
//成功读取文件后，把结果赋予img
reader.onload=function(evt){
img.src=evt.target.result;

}
reader.readAsDataURL(file.files[0]);
}

}

</script>
</head>
<body>
<h1>Hello, world!</h1>
<div>
<label for="advimg">广告图片</label><br>
<img id="image1" src="#" width="360" height="150">
    <!-- 当选中文件，将文件传给viewimg方法-->
<input type="file" id="advimg" name="advimg" onchange="viewimg(this)"> 
</div>

</body>
</html>
```

