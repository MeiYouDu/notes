# 特征、语法、规定
JSON 格式（JavaScript Object Notation 的缩写）是一种用于数据交换的文本格式，2001年由 Douglas Crockford 提出，目的是取代繁琐笨重的 XML 格式。
JSON 格式相比于xml有两个显著的优点：

- 书写简单，一目了然；符合 JavaScript 原生语法
- 可以由解释引擎直接处理，不用另外添加解析代码。

每个 JSON 对象就是一个值，可能是一个数组或对象，也可能是一个原始类型的值。总之，只能是一个值，不能是两个或更多的值。
JSON 对值的类型和格式有严格的规定。
:::info

1. 复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
2. 原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和null（不能使用NaN, Infinity, -Infinity和undefined）。
3. 字符串必须使用双引号表示，不能使用单引号。
4. 对象的键名必须放在双引号里面。
5. 数组或对象最后一个成员的后面，不能加逗号。
:::

# JSON.stringfy()
## 常规用法
```javascript
JSON.stringify('abc') // ""abc""
JSON.stringify(1) // "1"
JSON.stringify(false) // "false"
JSON.stringify([]) // "[]"
JSON.stringify({}) // "{}"

JSON.stringify([1, "false", false])
// '[1,"false",false]'

JSON.stringify({ name: "张三" })
// '{"name":"张三"}'
```
### 双引号
注意，对于原始类型的字符串，转换结果会带双引号。
```javascript
JSON.stringify('foo') === "foo" // false
JSON.stringify('foo') === "\"foo\"" // true
```
上面代码中，字符串`foo`，被转成了`"\"foo\""`。这是因为将来还原的时候，内层双引号可以让 JavaScript 引擎知道，这是一个字符串，而不是其他类型的值。
### 过滤
```javascript
var obj = {
  a: undefined,
  b: function () {}
};

JSON.stringify(obj) // "{}"
```
上面的代码中，undefined和function都会被`JSON.stringify()`过滤。
### 转成null
```javascript
var arr = [undefined, function () {}];
JSON.stringify(arr) // "[null,null]"
```
上面代码中，数组`arr`的成员是`undefined`和函数，它们都被转成了`null`。
### 空对象
```javascript
JSON.stringify(/foo/) // "{}"
```
### 不可遍历
```javascript
var obj = {};
Object.defineProperties(obj, {
  'foo': {
    value: 1,
    enumerable: true
  },
  'bar': {
    value: 2,
    enumerable: false
  }
});

JSON.stringify(obj); // "{"foo":1}"
```
上面代码中，`bar`是`obj`对象的不可遍历属性，`JSON.stringify`方法会忽略这个属性。
## 第二个参数
### 数组
`JSON.stringify`还有第二个参数，是一个数组，数组的元素指定哪些属性需要转化成字符串。
```javascript
var obj = {
  'prop1': 'value1',
  'prop2': 'value2',
  'prop3': 'value3'
};

var selectedProperties = ['prop1', 'prop2'];

JSON.stringify(obj, selectedProperties)
// "{"prop1":"value1","prop2":"value2"}"
```
### 函数
第二个参数还可以是个函数，用于改变每个`JSON.stringify`的返回值。
```javascript
function f(key, value) {
  if (typeof value === "number") {
    value = 2 * value;
  }
  return value;
}

JSON.stringify({ a: 1, b: 2 }, f)
// '{"a": 2,"b": 4}'
```
这个函数可以会被传入两个参数，分别是对象的键名和键值，注意这个函数会被递归调用处理所有的键。
```javascript
var obj = {a: {b: 1}};

function f(key, value) {
  console.log("["+ key +"]:" + value);
  return value;
}

JSON.stringify(obj, f)
// []:[object Object]
// [a]:[object Object]
// [b]:1
// '{"a":{"b":1}}'
```
如果处理函数返回`undefined`，或者没有返回值，则该属性会被忽略如下：
```javascript
function f(key, value) {
  if (typeof(value) === "string") {
    return undefined;
  }
  return value;
}

JSON.stringify({ a: "abc", b: 123 }, f)
// '{"b": 123}'
```
## 第三个参数
`JSON.stringify`还可以接受第三个参数，这个参数主要作用是帮助序列化字符串的时候格式化。增加生成的字符串的可读性。第三个参数类型可以是`string`可以是`number`。

- `string` 可以是`\n``\t`等，会在每个属性前插入。
- `number` 最大是10，表示属性前的空格数量。
```javascript
console.log(JSON.stringify({a: 1, b: 2, c: 3, d: 4, e: 1232131}, undefined, 2));
// {
//   "a": 1,
//   "b": 2,
//   "c": 3,
//   "d": 4,
//   "e": 1232131
// }
console.log(JSON.stringify({a: 1, b: 2, c: 3, d: 4, e: 1232131}, undefined, "aaa"));
// {
// 	aaa"a": 1,
// 	aaa"b": 2,
//  aaa"c": 3,
//  aaa"d": 4,
//  aaa"e": 1232131
// }
```
## 参数对象的toJSON()方法
`JSON.stringify()`在执行时，如果第一个参数具有`toJSON()`方法，那`JSON.stringify`方法会把`toJSON`方法执行的返回值作为自身执行的结果。
# JSON.parse()方法
`JSON.parse`方法将JSON字符串转化成对应的值。
```javascript
JSON.parse('{}') // {}
JSON.parse('true') // true
JSON.parse('"foo"') // "foo"
JSON.parse('[1, 5, "false"]') // [1, 5, "false"]
JSON.parse('null') // null

var o = JSON.parse('{"name": "张三"}');
o.name // 张三

// 如果传入的值不是有效的JSON就会报错
JSON.parse("'String'") // illegal single quotes
// SyntaxError: Unexpected token ILLEGAL
```
上面代码中的第11行，因为JSON中只能使用双引号，所以会报出错误，[JSON规则](#f6gmE)。
`JSON.parse`类似于`JSON.stringify`，第二个参数可以传入一个函数作为处理函数，用来处理每一个`key`，函数的返回值作为处理之后的`value`。
