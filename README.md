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
第一种方案最终效果如下:
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

### 第二种方案的功能分析(根据Photoshop功能分析):
>1. 左边色块从左到右为可见光光谱, 右边为明暗色带;
>2. 同时输出HSL和RGB值, 所以获取HSL后再转换为RGB: [HSL和RGB值互相转换](http://www.easyrgb.com/en/math.php)
>3. 右侧明暗发生变化时, 左边色块整体的颜色明暗也会随之发生变化;
>4. 左边色块中的圆形可以点击/拖拽选择颜色, 右边明暗带的三角形可以点击/拖拽改变明暗;


色带部分用如下css的linear-gradient实现：
```css
.color_wheel {
            /* 【红0%，黄17%，青34%，天蓝50%，蓝67%，紫84%，红100%】 */
            background: linear-gradient(to bottom, transparent, rgba(255, 255, 255, 1) 100%), linear-gradient(to right, rgb(255, 0, 0) 0%, rgb(255, 255, 0) 17%, rgb(0, 255, 0)34%, rgb(0, 255, 255) 50%, rgb(0, 0, 255) 67%, rgb(255, 0, 255) 84%, rgb(255, 0, 0) 100%);
        }
```
样式补充完成后效果如下：

![image](https://user-images.githubusercontent.com/84166052/118363804-b504a800-b5c8-11eb-9abc-9f5e2157a9e5.png)

接下来实现既可以点击选择颜色,又可以拖拽选色圆形的功能:
```javascript
        /* 下面实现: 点击后drag移动到鼠标位置 && 拖拽功能, 并且控制drag在色块区域内 */
        let color_wheel = document.querySelector('.color_wheel');
        let drag_circle = document.querySelector('.circle');
        //获取原点在页面中的坐标
        let basepointX = color_wheel.offsetLeft;
        let basepointY = color_wheel.offsetTop;
        //封装一个函数,控制drag在色块内
        function changeDragPosition(dragX, dragY) {
            if (dragX >= 0 && dragX <= 200) {
                drag_circle.style.left = dragX - 5 + 'px';
            }
            if (dragY >= 0 && dragY <= 250) {
                drag_circle.style.top = dragY - 5 + 'px';
            }
            if (dragX <= 0) {
                drag_circle.style.left = '0px';
            }
            if (dragY <= 0) {
                drag_circle.style.top = '0px';
            }
            if (dragX >= 190) {
                drag_circle.style.left = '190px';
            }
            if (dragY >= 240) {
                drag_circle.style.top = '240px';
            }
        }
        //点击时,drag移动到鼠标位置,6+4=10是圆drag的直径
        color_wheel.addEventListener('mouseup', function(event) {
                let dragX = event.pageX - basepointX;
                let dragY = event.pageY - basepointY;
                changeDragPosition(dragX, dragY);
            })
            //drag可以拖拽
        drag_circle.addEventListener('mousedown', function(event) {
            //当鼠标按下， 就获得鼠标在drag内的坐标
            let initX = (event.pageX - basepointX) - drag_circle.offsetLeft;
            let initY = (event.pageY - basepointY) - drag_circle.offsetTop;
            console.log(initX);
            //鼠标移动的时候，把鼠标在color_wheel中的坐标，减去 鼠标在drag内的坐标就是模态框drag的left和top值
            document.addEventListener('mousemove', moveDrag);

            function moveDrag(event) {
                let dragX = (event.pageX - basepointX) - initX + 5;
                let dragY = (event.pageY - basepointY) - initY + 5;
                console.log(dragX, dragY);
                changeDragPosition(dragX, dragY);
            }

            //鼠标弹起，就让鼠标移动事件移除
            document.addEventListener('mouseup', function() {
                document.removeEventListener('mousemove', moveDrag);
            })
        });
```

同理完成点击/拖拽三角形,控制明暗的功能;

接下来完成根据坐标,输出HSL和RGB颜色的功能:
观察可得容易从坐标得到HSL的值: 可见光色块部分,从左到右改变横坐标时SL不变,H(色相hue)从0°变为360°;从上到下改变纵坐标时, HL不变, S从100%变为0%; 所以,先获得HSL比较容易,最后转化成RGB一起输出.










