#前端积累

## js事件传播流程
 事件捕获阶段 ——》 处于目标阶段 ——》 事件冒泡阶段  
 事件冒泡：事件开始时由最具体的元素接收，然后逐级向上传播到较为不具体的元素  
 事件捕获：不太具体的节点更早接收事件，而最具体的元素最后接收事件，和事件冒泡相反  
## call、apply和bind的区别
 - call 是将方法中的this指向call中第一个参数，当第一个参数为null、undefined时，默认指向window; call中第一个参数之后是要传递给方法的参数列表
 - apply和call相似，不同之处在于传递给方法的参数形式不一致。apply传递给方法的参数是数组的形式。
 - bind不会执行方法，而是返回改变this指向后的新方法。
## 随机生成一个数组
 ```js
function sjsz(num){
    var ary = [];                    //创建一个空数组用来保存随机数组
    for(var i=0; i<num; i++){            //按照正常排序填充数组
        ary[i] = i+1;
    }
    ary.sort(function(){
        return 0.5-Math.random();        //返回随机正负值
    });
    return ary;                    //返回数组
}
sjsz(20)
```
## js数组去重
 数组递归去重  并排序
 ```js
 Array.prototype.Duplicate = function(){
     this.sort(function(a,b){  //对数组进行排序才能方便比较
         return a - b;
     });
     var re=[this[0]];
     for(var i = 1; i < this.length; i++) {
         if( this[i] !== re[re.length-1]) {
            re.push(this[i]);
         }
     }
    return re;
 }
 let seds=[1,5,18,1,3,115,17,10,2,5,7,3,8,7,1,5,8,41,2,89]
 console.log(seds.Duplicate());//[1, 2, 3, 5, 7, 8, 10, 17, 18, 41, 89, 115]
 ```  
 indexOf去重
 ```js
 Array.prototype.Duplicate = function(){
     var arr=[];/*临时数组*/
     for(let i=0;i<this.length;i++){//遍历当前数组
         if (arr.indexOf(this[i]) == -1) arr.push(this[i]);
     }
    return arr;
 }
 let seds=[1,5,18,1,3,115,17,10,2,5,7,3,8,7,1,5,8,41,2,89]
 console.log(seds.Duplicate());//[1, 5, 18, 3, 115, 17, 10, 2, 7, 8, 41, 89]
```
filter方法去重
```js
    function ser(){
        let arr=[1,5,18,1,3,115,17,10,2,5,7,3,8,7,1,5,8,41,2,89]
        arr.filter(function(element,index,self){
            console.log(self.indexOf(element) === index);
            return self.indexOf(element) === index;
        })
    }
    ser();
```
对象方法去重
```js
Array.prototype.Duplicate = function(){
    var n = {},r=[]; //n为hash表，r为临时数组
    for(var i = 0; i < this.length; i++) //遍历当前数组
    {
    if (!n[this[i]]) //如果hash表中没有当前项
    {
    n[this[i]] = true; //存入hash表
    r.push(this[i]); //把当前数组的当前项push到临时数组里面
    }
    }
    return r;
}
 let seds=[1,5,18,1,3,115,17,10,2,5,7,3,8,7,1,5,8,41,2,89];
console.log(seds.Duplicate());//[1, 5, 18, 3, 115, 17, 10, 2, 7, 8, 41, 89];

远行速度快，展内存大，空间换时间
```
es6的set方法
```js
function dedupe(array){
 return Array.from(new Set(array));
}
dedupe([1,5,18,1,3,115,17,10,2,5,7,3,8,7,1,5,8,41,2,89]) //[1, 5, 18, 3, 115, 17, 10, 2, 7, 8, 41, 89];
```
拓展运算符(...)内部使用for...of循环
```js
let arr =[1,5,18,1,3,115,17,10,2,5,7,3,8,7,1,5,8,41,2,89];
let resultarr = [...new Set(arr)]; 
console.log(resultarr); //[1, 5, 18, 3, 115, 17, 10, 2, 7, 8, 41, 89];
```
一个数组分成多个数组
```js
function group(array, subGroupLength) {
  let index = 0;
  let newArray = [];
  while(index < array.length) {
      newArray.push(array.slice(index, index += subGroupLength));
  }
  return newArray;
}
var Array = [1,5,18,1,3,115,17,10,2,5,7,3,8,7,1,5,8,41,2,89];
var groupedArray = group(Array, 2);
console.log(groupedArray)
```
## js合并数组并去重
concat()方法
```js
function concatArr(arr1, arr2){
  var arr = arr1.concat(arr2);
  arr = Duplicate(arr);//再引用上面的任意一个去重方法
  return arr;
}
var a = [1,5,18,1,3,115,17,10];
var b = [2,5,7,3,8,7,1,5,8,41,2,89];
concatArr(a,b)
```
数组对象的合并去重
```js
let json1=[{id:1,name:"aaa"},{id:2,name:"bbb"},{id:3,name:"ccc"},{id:5,name:"ggg"}] 
let json2=[{id:1,name:"aaa"},{id:2,name:"bbb"},{id:4,name:"ddd"},{id:5,name:"fff"}]
function concatArr(arr1, arr2){
  var json = arr1.concat(arr2);
  let newJson= []; //盛放去重后数据的新数组
  for(item1 of json){  //循环json数组对象的内容
  	let flag = true;  //建立标记，判断数据是否重复，true为不重复
  	for(item2 of newJson){  //循环新数组的内容
  		if(item1.id==item2.id){ //让json数组对象的内容与新数组的内容作比较，相同的话，改变标记为false
  			flag = false;
  		}
  	}
  	if(flag){ //判断是否重复
  		newJson.push(item1); //不重复的放入新数组。  新数组的内容会继续进行上边的循环。
  	}
  }
  return newJson;
}
concatArr(json1,json2);
```
## 有序数组变随机乱序
```js
var arr = [1, 2, 5, 6, 9, 16, 18, 21, 32] // 定义一个有序数组
arr.sort(function (a, b) { return Math.random() - 0.5 }) //每次执行结果都不一样，但乱序排序
```
## 数组倒序
```js
var arr = [1, 2, 5, 6, 9, 16, 18, 21, 32];
arr.sort(function (a, b) {return b-a;})
```

## filterr(滤镜)属性
 ```js
语法 filter: none | blur() | brightness() | contrast() | drop-shadow() | grayscale() | hue-rotate() | invert() | opacity() | saturate() | sepia() | url();
例子  ：img {
    -webkit-filter: grayscale(100%); /* Chrome, Safari, Opera */
    filter: grayscale(100%);
}
```
## 对象转为字符串
JSON.stringify(obj)
## json字符串转为对象
JSON.parse(str)
## 去除指定字符
```js
var str = "blog.123csdn.net";
console.log(str.replace("123",""));
replace(/[-]/g,"")//去除多个-
```
## position定位
+ position属性值
```js
static：元素默认值，即没有定位，遵循正常的文档流对象
relative：相对定位
absolute：绝对定位，相对于static以外的第一个父元素进行定位，搭配元素：left/right top/bottom
fixed：固定布定位，相对于浏览器窗口定位：搭配元素：left/right  top/bottom
inherit：规定应该从父元素继承 position 属性的值
sticky：css3新增，相当于relative和fixed结合，即滑动到一定程度就固定布局
参考链接：[http://www.liangxinghua.com/article/detail/21.html](http://www.liangxinghua.com/article/detail/21.html)
```
+ 定位机制
```composer log
控制元素布局的定位方案

1、普通流（normal flow）
HTML默认布局。
以HTML文档为基础，元素按照在HTML中出现的先后位置自上而下布局，内联元素水平排列，直到当行被沾满然后换行。
块级元素则会被渲染为完整的一个新行，除非另外制定，否则所有元素默认都是普通流定位。
也可以说，普通流中元素的位置由该元素在HTML文档中的位置决定。

2、浮动流（float flow）
在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能地向左边或右边偏移，
其效果与印刷排版中的文本环绕相似。

3、定位（positioning）
（1）绝对定位：absolute positioning
在绝对定位布局中，元素会整体脱离普通流，因此绝对定位元素不会对其兄弟元素造成影响，
而元素具体的位置由绝对定位的坐标决定。这种定位通过设置top、right、bottom、left这些偏移值，
相对于 static 定位以外的第一个父元素进行定位（这种定位通常设置父元素为relative定位来配合使用），
在没有父元素的条件下，它的参照为body，该方式脱离文档流；

（2）静态定位（static positioning）
当我们没有指定定位方式的时候，这时默认的定位方式就是static，也就是按照文档的书写布局自动分配在一个合适的地方，
这种定位方式用margin来改变位置，对left、top、z-index等设置值无效，这种定位不脱离文档流；

（3）相对定位（relative positioning）
该定位是一种相对的定位，可以通过设置left、top等值，使得指定元素相对其正常的位置进行偏移，这种定位不脱离文档流

（4）固定定位（fixed positioning）
生成绝对定位的元素，相对于浏览器窗口进行定位。这种定位方式是相对于整个文档的，只需设置它相对于各个方向的偏移值，
就可以将该元素固定在页面固定的位置，通常用来显示一些提示信息，脱离文档流；

（5）inherit定位
这种方式规定该元素继承父元素的position属性值。

注意：
float，absolute，fixed，脱离文档流，即将元素从普通的布局排版中拿走，
其他盒子在定位的时候，会当没看到它，两者位置重叠就会发生重叠
```
 
    