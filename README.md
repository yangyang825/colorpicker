# colorpicker

## 需求分析:
以下四种方案, 前三种为点击色块选择颜色, 第四种为滑动/拖拽选颜色, 所以分为两种实现类型-色块或者色带, 做出两种colorpicker.
![image](https://user-images.githubusercontent.com/84166052/118216523-40d1e380-b4a6-11eb-8b1f-1db36852c220.png)

## 第一种方案的颜色选择器: 点击色块选择颜色

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第一种方法:点击方块选择颜色</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        
        #colorpicker {
            width: 230px;
            height: 350px;
            margin: auto;
            padding: 2px;
            background-color: #f1f1f1;
            border: 1px solid black;
            text-align: center;
        }
        
        .color_container {
            display: flex;
            width: 220px;
            height: 210px;
            margin: 5px;
            flex-direction: row;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
            background-color: #cec7c7;
        }
        
        .rgb_value {
            margin: 10px;
        }
        
        .rgb_value input {
            display: inline-block;
            width: 70px;
        }
    </style>
</head>

<body>
    <div id="colorpicker">
        无颜色
        <div class="color_container"></div>
        <div class="rgb_value">
            RGB值为 <input type="text" value="">
        </div>
    </div>
    <script>
        let colorpicker = document.getElementById('colorpicker');
        let color_container = document.querySelector('.color_container');
        /* 创建颜色代码数组 */
        // 因为找不到这个案例的颜色变化的规律, 所以用了笨方法, 写死了颜色数组
        let color_array = [
            "#ffffff", "#e5e4e4", "#d9d8d8", "#c0bdbd", "#a7a4a4", "#8e8a8b", "#827e7f", "#767173", "#5c585a", "#000000",
            "#fefcdf", "#fef4c4", "#feed9b", "#fee573", "#ffed43", "#f6cc0b", "#f6cc0b", "#c9a601", "#ad8e00", "#8c7301",
            "#ffded3", "#ffc4b0", "#ff9d7d", "#ff7a4e", "#ff6600", "#e95d00", "#d15502", "#ba4b01", "#a44201", "#8d3901",
            "#ffd2d0", "#ffbab7", "#fe9a95", "#ff7a73", "#ff483f", "#fe2419", "#f10b00", "#d40a00", "#940000", "#6d201b",
            "#ffdaed", "#ffb7dc", "#ffa1d1", "#ff84c3", "#ff57ac", "#fd1289", "#ec0078", "#d6006d", "#bb005f", "#9b014f",
            "#fcd6fe", "#fbbcff", "#f9a1fe", "#f784fe", "#f564fe", "#f546ff", "#f328ff", "#d801e5", "#c001cb", "#8f0197",
            "#e2f0fe", "#c7e2fe", "#add5fe", "#92c7fe", "#6eb5ff", "#48a2ff", "#2690fe", "#0162f4", "#013add", "#0021b0",
            "#d3fdff", "#acfafd", "#7cfaff", "#4af7fe", "#1de6fe", "#01deff", "#00cdec", "#01b6de", "#00a0c2", "#0084a0",
            "#edffcf", "#dffeaa", "#d1fd88", "#befa5a", "#a8f32a", "#8fd80a", "#79c101", "#3fa701", "#307f00", "#156200",
            "#d4c89f", "#daad88", "#c49578", "#c2877e", "#ac8295", "#c0a5c4", "#969ac2", "#92b7d7", "#80adaf", "#9ca53b",
        ];

        // 遍历数组，添加对应颜色的小方块
        for (let color of color_array) {
            color_container.innerHTML += `<div value="${color}" style="background: ${color}; width: 15px; height: 15px; margin: 3px; cursor: pointer;"></div>`
        }


        //加点击事件
        let color_squares = document.querySelectorAll('.color_square');
        let rgb_value = document.querySelector('.rgb_value').querySelector('input');
        color_container.addEventListener('click', function(event) {
            //console.log(event.target);
            //div直接获取name和value属性获取不到, 得用getAttribute()
            rgb_value.value = event.target.getAttribute('value');
        })
    </script>
</body>

</html>
```
效果如下:
![image](https://user-images.githubusercontent.com/84166052/118217216-93f86600-b4a7-11eb-8c35-e607363ec6e3.png)

### 进行了改进, 把整个颜色选择器封装到js文件里, 通过appendChild的形式添加盒子和样式, 使用的时候就只需要添加js文件即可:
```javascript
        /* 1. 整体的父盒子部分 */
        let colorpicker = document.createElement("div");
        colorpicker.style = `
        width: 230px;
        height: 300px;
        margin: auto;
        padding: 2px;
        background-color: #f1f1f1;
        border: 1px solid black;
        text-align: center;
        `;
        colorpicker.innerText = '点击选择颜色';

        /* 2. 选色盒子部分 */
        let color_container = document.createElement("div");
        color_container.style = `
        display: flex;
        width: 220px;
        height: 210px;
        margin: 5px;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
        background-color: #cec7c7;
        `;

        /* 创建颜色代码数组 */
        let color_array = [
            "#ffffff", "#e5e4e4", "#d9d8d8", "#c0bdbd", "#a7a4a4", "#8e8a8b", "#827e7f", "#767173", "#5c585a", "#000000",
            "#fefcdf", "#fef4c4", "#feed9b", "#fee573", "#ffed43", "#f6cc0b", "#f6cc0b", "#c9a601", "#ad8e00", "#8c7301",
            "#ffded3", "#ffc4b0", "#ff9d7d", "#ff7a4e", "#ff6600", "#e95d00", "#d15502", "#ba4b01", "#a44201", "#8d3901",
            "#ffd2d0", "#ffbab7", "#fe9a95", "#ff7a73", "#ff483f", "#fe2419", "#f10b00", "#d40a00", "#940000", "#6d201b",
            "#ffdaed", "#ffb7dc", "#ffa1d1", "#ff84c3", "#ff57ac", "#fd1289", "#ec0078", "#d6006d", "#bb005f", "#9b014f",
            "#fcd6fe", "#fbbcff", "#f9a1fe", "#f784fe", "#f564fe", "#f546ff", "#f328ff", "#d801e5", "#c001cb", "#8f0197",
            "#e2f0fe", "#c7e2fe", "#add5fe", "#92c7fe", "#6eb5ff", "#48a2ff", "#2690fe", "#0162f4", "#013add", "#0021b0",
            "#d3fdff", "#acfafd", "#7cfaff", "#4af7fe", "#1de6fe", "#01deff", "#00cdec", "#01b6de", "#00a0c2", "#0084a0",
            "#edffcf", "#dffeaa", "#d1fd88", "#befa5a", "#a8f32a", "#8fd80a", "#79c101", "#3fa701", "#307f00", "#156200",
            "#d4c89f", "#daad88", "#c49578", "#c2877e", "#ac8295", "#c0a5c4", "#969ac2", "#92b7d7", "#80adaf", "#9ca53b",
        ];

        // 遍历数组，添加pick按钮至容器
        for (let color of color_array) {
            color_container.innerHTML += `<div value="${color}" style="background: ${color}; width: 15px; height: 15px; margin: 3px; cursor: pointer;"></div>`
        }


        /* 3. 显示颜色RGB数值的部分 */
        let show_rgb = document.createElement('div');
        show_rgb.innerHTML = `RGB值为 <input type="text" value="">`;


        /* 4. 最后加上点击事件 */
        let rgb_value = show_rgb.querySelector('input');
        color_container.addEventListener('click', function(event) {
            console.log(event.target);
            //div直接获取name属性获取不到, 得用getAttribute()
            rgb_value.value = event.target.getAttribute('value');
        })

        /* append进body里 */
        colorpicker.appendChild(color_container);
        colorpicker.appendChild(show_rgb);
        document.body.appendChild(colorpicker);
```

## 第二种方案颜色选择器：拖拽或者点击色带进行选择
![image](https://user-images.githubusercontent.com/84166052/118252281-2a924a80-b4db-11eb-9136-d95579b9d3c1.png)
这种渐变的色带容易想到用css3的[linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient())直接实现渐变，不过如果要把功能都放进一个js文件里方便移植，还可以用js来填渐变色; 从左到右是基本色渐变，从上到下是从明到暗渐变。

难点还是找可见光颜色渐变的规律，查询相关资料和取色找规律得出渐变规律。

最后的显示数值部分，除了显示RGB值，还有HSV值，所以还涉及到两种数值的转换规则。

>[MDN linear-gradient](https://developer.mozilla.org/zh-CN/docs/Web/CSS/linear-gradient())
>
>[色带的RGB值变化规律，配合取色找出规律](https://zh.wikipedia.org/wiki/%E5%8F%AF%E8%A7%81%E5%85%89)
>
>[HSV和RGB值互相转换](https://blog.csdn.net/lly_3485390095/article/details/104570885)








