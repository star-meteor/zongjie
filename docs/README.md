# 获取表单数据的发热的v

- jquery提供的$('form').serialize()方法来获取表单中的所有数据
  - $('form').serialize();根据表单各项的name属性获取
  - 返回值：一个类似于"username=xxx&password=yyyy"
- jquery提供的$('form').serializeArray()方法来获取表单中的所有数据
  - 根据表单的name属性获取
  - 返回值：一个数组，类似于【{name:'admin'},{password:'123456'}】
- FormData()对象获取表单中的所有数据
  - 根据表单各项的name属性获取
  - 返回值：对象，属性名为上面的表单name的属性值，属性值为表单的内容
  - 通过实例化对象.append('属性'，‘属性值)，增加属性
  - 需要用$.ajax()；里面设置process:false;contentType:false;

![1566960120964](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1566960120964.png)

- DOM对象.dataset;

  作用：获取某个DOM节点的data-xxx属性

  返回值：返回的是{xxx:属性值}

  ![1566976833016](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1566976833016.png)

# 密码加密

- md5函数

  ```js
  $('.register').click(function () {
        // 收集表单数据
         var data = $('form').serializeArray();
        data[1].value = md5(data[1].value); // 这是密码
         //data为数组【{}{}】  
        // 将数据发送给注册的接口，约定接口为 /reg
        $.post('/reg', data, function (res) {
          alert(res.message);
          if (res.code == 200) {
            // 注册成功，跳转到登录页
            location.href = '/login.html';
          }
        }, 'json');
      });
  ```

  



# 跳转

location.href='';    

# 刷新页面

location.reload();

# 弹出框

- confirm()
  - 有一个返回值，用户如果点击确定，该函数返回true；用户点击取消，该函数返回false
- alert（）
  - 只是一个提示性信息，只能点确定
- prompt()
  - 输入框：用户可以输入相应的信息

# 从地址栏获取参数

- location.search属性

  ```js
  var id = location.search.substr(4);或
  var id=location.search.replace(/\D/g, '');
  ```

- 实例化内置对象URL（随意的文件+location.search,随意的地址）

  获得相应的属性值：location.searchParams.get(属性)

  ```js
   var url = new URL('aa.html' + location.search, 'http://www.ss.com');
   var id = url.searchParams.get('id');
   var categoryId = url.searchParams.get('categoryId');
  ```

  

# 实现图片本地预览

```js
//当文本域的内容发生改变的时候,实现预览
$('input').change(function(){
    //1.找到文件对象
    var fileObj=this.files[0];
    //2.根据文件对象，创建一个临时的url,url用于访问这张图片
    var url=URL.createObjectURL(fileObj);
    //3.设置预览图片的src,只就是临时的url
    $('img').prop('src',url);    
})
```

# 将number数据变为字符串的方法

Data.now()+'';

# 解决get缓存,无法更新的问题

 在链接的后面加时间戳

```js
this.src="'/captcha?t='+Date.now()"
```

# 分页和模糊查询

首先引入jquery.pagination.css和jquery.pagination.min.js

```js
//放分页的div
<div id="pagination" class="page"></div>

//script代码
let params = {
      page: 1,
      keywords: null
    }
    // 发送ajax请求到 getHeroes 接口，获取所有的英雄，并将数据渲染到页面中
    function loadData() {
      $.get('/getHeroes', params, function (res) {
        console.log(res);
        var str = template('moban', {
          arr: res.data
        });
        $('tbody').html(str);

        page(params.page, res.totalPage);
      }, 'json');
    }
    loadData();

    ///////////////////// 下面是实现分页的函数 /////////////////////////
    function page(c, t) {
      $("#pagination").pagination({
        currentPage: c, // 表示开始显示第几页
        totalPage: t, // 表示总页数
        callback: function (current) {
          // alert('ok!');
          // alert(current); // 表示当前被点击的页码
          // 改变 params.page 的值
          params.page = current;
          // 重写调用接口，获取数据，从新将数据渲染到页面中
          loadData();
        }
      });
    }

    ///////////////////////////////////////////
    // 模糊搜索
    $('.btn-default').click(function (e) {
      e.preventDefault();
      // 获取输入框的值
      var keywords = $('input[name="search"]').val();
      params.keywords = keywords;
      params.page = 1; // 搜索完毕，应该也是查看第一页
      loadData();
    });
  </script>

```

# 数组中reverse方法

作用：颠倒数组中元素的顺序

语法：arrayObject.reverse();

```js
var arr=[1,2,3];
log(arr.reverse());
```

# 字符串方法

## 01.将字符串转为大写或小写

### 转为大写

- 语法stringObject.toUpperCase()
- 返回值：一个新的字符串，在其中 stringObject 的所有小写字符全部被转换为了大写字符

​	

### 转为小写

- 语法：stringObject.toLowerCase()
- 返回值：一个新的字符串，在其中 stringObject 的所有大写字符全部被转换为了小写字符。

## 02. 查找

- 语法stringObject.charAt(index)

  - 作用：返回在指定位置的一个字符
  - 参数index:必须，字符在字符串中的下标
  - **注释：**字符串中第一个字符的下标是 0。如果参数 index 不在 0 与 string.length 之间，该方法将返回一个空字符串

- stringObject.indexOf(指定字符)

  - 作用：字符串中是否存在指定字符 存在就返回找到的下标，没有就-1；

  ```
  // 这个方法用于查找某个字符串是否包含于另一个字符串
  var str = '我爱中华人民共和国';
  console.log(str.indexOf('中华'));
  
  ```

- stingObject.lastIndexOf  用法和indexOf一样，只不过是从后面开始找

- stingObject.charCodeAt(指定字符的位置)

  - 作用：这个方法用于获取字符串中位于指定位置的字符的ASCII码

    ```js
    // 这个方法用于获取字符串中位于指定位置的字符的ASCII码
    var str = 'abcdef'
    console.log(str.charCodeAt(0));
    
    ```

    

## 03.连接两个或多个字符串

- 语法：stringObject.concat(stringX,stringX,....,stringX)
  - 参数：stringX:必须，将被连接为一个字符串的一个或多个字符串对象
- 返回值：	

## 04.提取字符串的片断 (截取)与拼接

### stringObject.substr(start,length)

- 作用：substr() 方法可在字符串中抽取从 *start* 下标开始的指定数目的字符。
- 参数star:必需。要抽取的子串的起始下标。必须是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。
- 参数length:可选。子串中的字符数。必须是数值。如果省略了该参数，那么返回从 *stringObject* 的开始位置到结尾的字串。
- 返回值：一个新的字符串，包含从 *stringObject* 的 *start*（包括 start 所指的字符） 处开始的 *length* 个字符。如果没有指定 *length*，那么返回的字符串包含从 *start* 到 *stringObject* 的结尾的字符。

### stringObject.slice(start,end)左包右不包

- 作用：提取两个指定位置之间字符串的片断，并在新的字符串中返回被提取的部分
- 参数start:要抽取的片断的起始下标。如果是负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推。
- 参数end:紧接着要抽取的片段的结尾的下标。若未指定此参数，则要提取的子串包括 start 到原字符串结尾的字符串。如果该参数是负数，那么它规定的是从字符串的尾部开始算起的位置。
- 返回值：一个新的字符串。包括字符串 stringObject 从 start 开始（包括 start）到 end 结束（不包括 end）为止的所有字符。

### stringObject.substring(start,stop)左包右不包

- 作用：提取字符串中两个指定的索引号之间的字符。

- 参数：start:必需。一个非负的整数，规定要提取的子串的第一个字符在 stringObject 中的位置。

- 参数：end:可选。一个非负的整数，比要提取的子串的最后一个字符在 stringObject 中的位置多 1。

  如果省略该参数，那么返回的子串会一直到字符串的结尾。

- 返回值：一个新的字符串，该字符串值包含 *stringObject* 的一个子字符串，其内容是从 *start* 处到 *stop*-1 处的所有字符，其长度为 *stop* 减 *start*。

### slice().substring(),substr()的区别

String 对象的方法 slice()、substring() 和 substr() （不建议使用）都可返回字符串的指定部分。slice() 比 substring() 要灵活一些，因为它允许使用负数作为参数。slice() 与 substr() 有所不同，因为它用两个字符的位置来指定子串，而 substr() 则用字符位置和长度来指定子串。

### 拼接

arrayObject.concat(要拼接的一个或多个字符串，用逗号相隔)

作用：返回新的字符串

```js
// 这个方法用于连接多个字符串，其作用相当于 + 操作符
var res = "abc".concat('def','ghi');
console.log(res);

```

## 05.把一个字符串分割成字符串数组

- 语法：stringObject.split(separator,howmany)
  - 参数：separator：必需。字符串或正则表达式，从该参数指定的地方分割 stringObject。（分割的字符，不写默认，）
  - 参数：howmany：可选。该参数可指定返回的数组的最大长度。如果设置了该参数，返回的子串不会多于这个参数指定的数组。如果没有设置该参数，整个字符串都会被分割，不考虑它的长度。

## 06.返回字符串

语法：stringObject.toString()

返回值：stringObject 的原始字符串值。一般不会调用该方法。

## 07.删除字符串两侧的空白

stringObject.trim()

```js
//让用户不能输入空白字符串
//接收用户写的字符串
var str=input.value;
var arr=str.split(' ');
var str=arr.join('');
var str=str.trim();

```



# 数组

## 01. 对元素进行操作

- ArrayObject.push()

  作用：从数组后面添加数据，参数为一个或多个，逗号隔开

  返回值：修改后数组的长度

- ArrayObject.unshift()

  作用：从数组的前面添加一个或多个元素，逗号隔开

  返回值：修改后数组的长度

- ArrayObject.pop()

  作用：删除数组最后一个元素，不需要参数，

  返回值：被删除的元素

- ArrayObject.shift()

  作用：将数组的第一个元素移除，

  返回值：被删除的元素

- splice：可进行数组任何位置的增删改，返回值是被移除的数组

  - 删除

    数组名.splice(从索引几开始移除,移除几个数据），返回值：**被移除的数组**;例：arr.splice(3,1);

  - 增加：

    数组名.splice(从索引几开始移除,移除几个数据，添加的数据），返回值：空数组;例：arr.splice(3,0，7，8)；注意：第四个数据就是7

  - 改

    数组名.splice(从索引几开始移除,移除几个数据，替换的数据），返回值：被移除的数组;例：arr.splice(3,1，“a”);

    ```js
      // 数组的splice方法用于从数组的指定位置移除、添加、替换元素
      var arr = ['a','b','c','d','e'];
      // 对原数组操作
      // 作用：从索引3开始移除，总共移除1个元素 ，
      // 返回 被移除元素的数组
      arr.splice(3,1); 
      console.log(arr);
    
    // 在c的后面添加7和8两个元素
        // 作用：从索引3开始添加，移除0个元素，把7，8加入；
        // 返回：一个空数组
        // 操作原数组；
        arr.splice(3,0,7,8); 
       
    
    // 作用：从索引1开始替换，总共替换1个，用0替换 ；
        // 返回：被替换元素的数组
        arr.splice(1,1,0);
        console.log(arr); 
    
    ```

   

## 02.将数组变为字符串 

语法： arrayObject.join(separator)

参数：separator:可选。指定要使用的分隔符。如果省略该参数，则使用逗号作为分隔符

返回值：返回一个字符串。该字符串是通过把 arrayObject 的每个元素转换为字符串，然后把这些字符串连接起来，在两个元素之间插入 *separator* 字符串而生成的。

## 03.查找元素

- ArrayObject.indexOf(被查找元素)

  作用：根据元素查找索引，如果这个元素在数组中，返回索引，否则返回-1

  作用：找元素在不在数组内部

- ArrayObject.findIndex（function(item){}）

  作用：查找满足条件的第一个元素的索引，如果没有，返回-1

  - 返回满足条件的第一个元素的索引
  - 参数为函数

## 04.遍历数组

- for：JS基础循环
- arrayObject.forEach(function(item,index,arr))方法：遍历数组

```js
// 数组的forEach方法用于遍历数组
var arr_1 = [10,20,30,40,50];

// forEach方法需要一个参数是一个函数，这个函数也接收3个参数，
// item是数组中的每个元素，index是item的索引,arr表示当前数组；
// 对本数组操作，没有返回值；

var sum=0;
arr_1.forEach(function(item,index,arr){
  //console.log(item,index);
    //求和
    sum+=item;
});


```

- arrayObject.filter (function(item,index,arr))筛选出数组中满足条件的数组，**返回是一个新的数组；**

```js
// 数组的filter方法用于将数组中满足条件的元素筛选出来
// 筛选出数组中 小于 2000 的数据
var arr_1 = [1500,1800,2200,300,2600,800];
var res = arr_1.filter(function(item,index,arr)
 //return后面需要跟一个筛选条件
 //条件表达式，需要布尔值
  return item < 2000;
});
// fitler方法的的参数要求是一个函数，这个函数接收2个参数：item是数组中的每个元素，index是item对应的索引,arr代表当前的数组

// 复制一个数组
var arr_1 = arr_2.filter(function(item,index,arr){
  return item;
});


```

- arrrayObject.map(item,index,arr)

  参数：map中回调函数中的第一个参数为：当前正在遍历的元素

  ​          map中回调函数中的第二个参数为：当前元素索引

  ​           map中回调函数第三个参数为：原数组本身

  ​    返回值：返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组

## 05.拼接与截取

- arrayObject.concat()

  作用：把单个数据或多个数组合并成一个新的数组

  返回值：不改变原数组，创建新的数组返回

  ```js
  // 数组的concat方法的作用是把单个数据或多个数组合并成一个新的数组
  var arr1 = [1,2,3];
  var arr2 = [4,5,6];
  var arr3 = [7,8,9];
  var res = arr1.concat(arr2,arr3);
  console.log(res);
  
  // 复制一个数组
  var arr_1 = arr.concat();
  
  ```

- arrayObject.slice（从索引几开始，截取到索引几）左包右不包

  slice截取数组：不对原数组操作，

  返回值：新的数组

  ```js
  var arr = ['a','b','c','d','e'];
  // 表示 从下标1（包括），截取到下表为4(不包括),
  var res = arr.slice(1, 4);
  
  // 如果不给第二个参数，默认就是把从start开始，到length结束的所有的元素截取
  var arr_1 = arr_2.slice(1);
  
  // 复制数组：如果省略两个参数，start默认是0，end默认是length
  var arr_1 = arr_2.slice();
  
  ```

## 06.数组复制

```js
//不能直接进行赋值

var arr = [10,20,30,40,50];




var new_arr = [];
arr.forEach(function(item,index,arr){
  new_arr.push(item)
});

var new_arr = arr.filter(function(item,index,arr){
  return item;
});

var new_arr = arr.concat();

var new_arr = arr.slice();
 

```



