# ES6  
### 1. 解构赋值  
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。  
```
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```  
如果解构不成功，变量的值就等于undefined。  
另一种情况是不完全解构，即等号左边的模式，只匹配一部分的等号右边的数组。这种情况下，解构依然可以成功。  
如果等号的右边不是数组，那么将会报错。  
  
解构赋值允许指定默认值。  
```
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```  
只有当一个数组成员严格等于undefined，默认值才会生效。  
  
默认值可以引用解构赋值的其他变量，但该变量必须已经声明。  
```
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError: y is not defined
```
对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。  
```
let { bar, foo } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: 'aaa', bar: 'bbb' };
baz // undefined
```
解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。  
### 2.字符串的扩展  
JavaScript 共有 6 种方法可以表示一个字符。  
```
'\z' === 'z'  // true
'\172' === 'z' // true
'\x7A' === 'z' // true
'\u007A' === 'z' // true
'\u{7A}' === 'z' // true
```
ES6 为字符串添加了遍历器接口，使得字符串可以被for...of循环遍历。  
模板字符串（template string）是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。  
如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。  
```
let greeting = `\`Yo\` World!`;
```
模板字符串中嵌入变量，需要将变量名写在${}之中。  
### 3.字符串新增方法  
1. String.fromCodePoint()方法可以识别大于0xFFFF的字符:
```
String.fromCodePoint(0x20BB7)
// "ஷ"
```
2. codePointAt()方法能够正确处理 4 个字节储存的字符，返回一个字符的码点:
```
let s = '𠮷a';

s.codePointAt(0) // 134071
s.codePointAt(1) // 57271
s.codePointAt(2) // 97
```

includes()：返回布尔值，表示是否找到了参数字符串。  
startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。  
endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。  
这三个方法都支持第二个参数，表示开始搜索的位置。  
```
let s = 'Hello world!';

s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```  
3. repeat方法返回一个新字符串，表示将原字符串重复n次。  
ES2017 引入了字符串补全长度的功能，如果某个字符串不够指定长度，会在头部或尾部补全。padStart()用于头部补全，padEnd()用于尾部补全。  
```
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```
4. ES2019 对字符串实例新增了trimStart()和trimEnd()这两个方法。  
它们的行为与trim()一致，trimStart()消除字符串头部的空格，trimEnd()消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。
```
const s = '  abc  ';

s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```
5. ES2021 引入了replaceAll()方法，可以一次性替换所有匹配。  
字符串的实例方法replace()只能替换第一个匹配。
```
'aabbcc'.replaceAll('b', '_')
// 'aa__cc'
```
它的用法与replace()相同，返回一个新字符串，不会改变原字符串。  
### 4.数值的扩展  
ES6 提供了二进制和八进制数值的新的写法，分别用前缀0b（或0B）和0o（或0O）表示。  
如果要将0b和0o前缀的字符串数值转为十进制，要使用Number方法。  
```
Number('0b111')  // 7
Number('0o10')  // 8
```
ES2021，允许 JavaScript 的数值使用下划线（_）作为分隔符。这个数值分隔符没有指定间隔的位数，也就是说，可以每三位添加一个分隔符，也可以每一位、每两位、每四位添加一个。小数和科学计数法也可以使用数值分隔符。  
  
ES6 在Number对象上，新提供了Number.isFinite()和Number.isNaN()两个方法。Number.isFinite()用来检查一个数值是否为有限的（finite），即不是Infinity。注意，如果参数类型不是数值，Number.isFinite一律返回false。  
Number.isNaN()用来检查一个值是否为NaN。如果参数类型不是NaN，Number.isNaN一律返回false。  
  
ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。
  
Number.isInteger()用来判断一个数值是否为整数。  
JavaScript 采用 IEEE 754 标准，数值存储为64位双精度格式，数值精度最多可以达到 53 个二进制位（1 个隐藏位与 52 个有效位）。如果数值的精度超过这个限度，第54位及后面的位就会被丢弃，这种情况下，Number.isInteger可能会误判。  
  
ES6 在Number对象上面，新增一个极小的常量Number.EPSILON。根据规格，它表示 1 与大于 1 的最小浮点数之间的差。对于 64 位浮点数来说，大于 1 的最小浮点数相当于二进制的1.00..001，小数点后面有连续 51 个零。这个值减去 1 之后，就等于 2 的 -52 次方。  
  
JavaScript 能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表示这个值。代码中超出 2 的 53 次方之后，一个数就不精确了。ES6 引入了Number.MAX_SAFE_INTEGER和Number.MIN_SAFE_INTEGER这两个常量，用来表示这个范围的上下限。  
Number.isSafeInteger()则是用来判断一个整数是否落在这个范围之内。  
  
ES2020 引入了一种新的数据类型 BigInt（大整数），来解决这个问题，这是 ECMAScript 的第八种数据类型。BigInt 只用来表示整数，没有位数的限制，任何位数的整数都可以精确表示。为了与 Number 类型区别，BigInt 类型的数据必须添加后缀n。另外，取反运算符（!）也可以将 BigInt 转为布尔值。  
  
### 5. 函数的扩展  
ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。  
参数变量是默认声明的，所以不能用let或const再次声明。  
```
function foo(x = 5) {
  let x = 1; // error
  const x = 2; // error
}
```
上面代码中，参数变量x是默认声明的，在函数体中，不能用let或const再次声明，否则会报错。  
使用参数默认值时，函数不能有同名参数。  
  
  ### 6. 对象的扩展  
  
