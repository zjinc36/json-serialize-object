##  项目来源
本项目是基于这个仓库的代码修改的[https://github.com/macek/jquery-serialize-object](https://github.com/macek/jquery-serialize-object)


#   API
##  引入jquery.js和jquery-serialize-object.js
```
<head>
  <script src="jquery.min.js"></script>
  <script src="jquery.serialize-object.min.js"></script>
</head>
```
##  给定html基本形式
```html
    <form id="contact">
        <input type="text" name="storeInfo[p_1][storeNm][0]" value="A" />
        <input type="text" name="storeInfo[p_1][storeNm][0]" value="B" />
        <input type="text" name="storeInfo[p_1][storeNm][0]" value="C" />
        <input type="text" name="storeInfo[p_1][storeNm][0]" value="D" />
    </form>
```

##  使用
.serializeObject ---- 将所选表单序列化为JavaScript对象
```javascript
$('form#contact').serializeObject();
```

.serializeJSON ---- 将所选表单序列化为JSON
```javascript
$('form#contact').serializeJSON();
```

上述表单转换成json的结果如下(为了阅读方便,我有进行格式化):
```
{
    "storeInfo":[
        {
            "storeNm":[
                "A"
            ]
        },
        {
            "storeNm":[
                "B"
            ]
        },
        {
            "storeNm":[
                "C"
            ]
        },
        {
            "storeNm":[
                "D"
            ]
        }
    ]
}
```

##  有以下几种类型
1.  push - push一个值到一个数组,底下的`p_1`是`p_数字`的形式
```html
<input name="fop[p_1]" value="a">
<input name="foo[p_1]" value="b">
```
$("form").serializeObject();
//=> {foo: [a, b]}

2.  fixed — add a value to an array at a specified index
```html
<input name="foo[2]" value="a">
<input name="foo[4]" value="b">
```
$("form").serializeObject();
//=> {foo: [, , "a", , "b"]}

3.  named — adds a value to the specified key
```html
<input name="foo[bar]" value="a">
<input name="foo[bof]" value="b">
<input name="hello" value="world">
```
$("form").serializeObject();
//=> {foo: {bar: "a", bof: "b"}, hello: "world"}
