# mock

**写在最前面**

如果是想学`mock`请移步[移步官网mock](http://mockjs.com/)，本文章只是我**自己学习的笔记**

工作需要，被动学习。

## mock的作用
`Mock.js`是一个模拟数据生成器，可以帮助前端开发和原型分离后端进度，并减少一些单调，特别是在编写自动化测试时。

## mock 安装

### 基本语法


- rurl: url **(可选)**
- rtype: 请求类型(get/post) **(可选)**
- template|function( options ): 模板/方法 **(可选)**

```javascript {.line-numbers}
Mock.mock( rurl?, rtype?, template|function( options ))
```

### 通过cdn安装

```javascript {.line-numbers}
<script src="http://mockjs.com/dist/mock.js"></script>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script>
// Mock.mock( template )
// 官网文档扣下来的模板

var template = {
    'title': 'Syntax Demo',

    'string1|1-10': '★',
    'string2|3': 'value',

    'number1|+1': 100,
    'number2|1-100': 100,
    'number3|1-100.1-10': 1,
    'number4|123.1-10': 1,
    'number5|123.3': 1,
    'number6|123.10': 1.123,

    'boolean1|1': true,
    'boolean2|1-2': true,

    'object1|2-4': {
        '110000': '北京市',
        '120000': '天津市',
        '130000': '河北省',
        '140000': '山西省'
    },
    'object2|2': {
        '310000': '上海市',
        '320000': '江苏省',
        '330000': '浙江省',
        '340000': '安徽省'
    },

    'array1|1': ['AMD', 'CMD', 'KMD', 'UMD'],
    'array2|1-10': ['Mock.js'],
    'array3|3': ['Mock.js'],

    'function': function() {
        return this.title
    }
}
var data = Mock.mock(template)

$('<pre>').text(JSON.stringify(data, null, 4)).appendTo('body')
</script>
```
### npm安装

```javascript {.line-numbers}
const Mock = require('mockjs');
var obj = {'aa':'11', 'bb':'22', 'cc':'33', 'dd':'44'};

// Mock响应模板
Mock.Mock(/\.json/, {
    "user|1-3": [{   // 随机生成1到3个数组元素
        'name': '@cname',  // 中文名称
        'id|+1': 88,    // 属性值自动加 1，初始值为88
        'age|18-28': 0,   // 18至28以内随机整数, 0只是用来确定类型
        'birthday': '@date("yyyy-MM-dd")',  // 日期
        'city': '@city(true)',   // 中国城市
        'color': '@color',  // 16进制颜色
        'isMale|1': true,  // 布尔值
        'isFat|1-2': true,  // true的概率是1/3
        'fromObj|2': obj,  // 从obj对象中随机获取2个属性
        'fromObj2|1-3': obj,  // 从obj对象中随机获取1至3个属性
        'brother|1': ['jack', 'jim'], // 随机选取 1 个元素
        'sister|+1': ['jack', 'jim', 'lily'], // array中顺序选取元素作为结果
        'friends|2': ['jack', 'jim'] // 重复2次属性值生成一个新数组
    },{
        'gf': '@cname'
    }]
});
```

**通过 get 请求**

打印出对应数据

```javascript {.line-numbers}
$.ajax({
    url: /\.json/,
    type: 'get',
    dataType: 'json'
})
.then(res =>{ss
    console.log(res);
})
```

#### 设置 post、带参数的请求

响应时也可以是使用 `function`, 如:

```javascript {.line-numbers}
const Mock = require('mockjs');
Mock.mock(/\.json/, 'post', function(options) {
    return options.type
})
```

##### 通过 post 请求

```javascript {.line-numbers}
$.ajax({
    url: 'hello.json',
    type: 'post',
    dataType: 'json'
})
.then(res()=>{
    console.log(res);
})
```

##### 通过 post 带参数请求

```javascript {.line-numbers}
$.ajax({
    url: 'hello.json',
    type: 'post',
    dataType: 'json'
    data: {
      account: 888,
      pwd: 'abc123'
    }
})
.then(res()=>{
    console.log(res);
})

const Mock = require('mockjs');
Mock.mock(/\.json/, 'post', function(options) {
    console.log(options);
    // 打印出 {url: "http://test.com", type: "POST", body: "account=888&pwd=abc123"}
    return Mock.mock({
        // .....
    });
});
```
