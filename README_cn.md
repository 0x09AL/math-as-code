# math-as-code

> 译注：英文不好尝试翻译，如有错误请指正。

这是一份通过对比数学符号和JavaScript代码来帮助开发者了解数学符号的参考。

动机:学术论文可能会吓着自学游戏和图形的程序猿:)

这个指南还没有完成。如果你发现错误或者想提交贡献，请[open a ticket](https://github.com/Jam3/math-as-code/issues)或发一个 PR。

> **Note**: For brevity, some code examples make use of [npm packages](https://www.npmjs.com/). You can refer to their GitHub repos for implementation details.

# 前言

数学符号可以表示不同的意思，这取决于作者，上下文和所学习的领域（线性代数，集合理论，等等）。这份指南也许不会涵盖符号的*所有*用法。在某些情况，会引用一些真实材料（博客文章,出版物等等）来演示某个符号的用法。

更完整的列表，请看[Wikipedia - List of Mathematical Symbols](https://en.wikipedia.org/wiki/List_of_mathematical_symbols). 

简单起见，这里许多的代码示例都操作浮点数值，并不是数字健壮的(numerically robust)。为什么这会是一个问题的更详细信息请看[Robust Arithmetic Notes](https://github.com/mikolalysenko/robust-arithmetic-notes) by Mikola Lysenko.

# 目录

- [变量名惯例](#variable-name-conventions)
- [等号 `=` `≈` `≠` `:=`](#equals-symbols)
- [平方根与复数 `√` *`i`*](#square-root-and-complex-numbers)
- [点 & 叉 `·` `×` `∘`](#dot--cross)
  - [标量乘法](#scalar-multiplication)
  - [向量乘法](#vector-multiplication)
  - [点乘](#dot-product)
  - [叉乘](#cross-product)
- [sigma `Σ`](#sigma) - *summation*
- [大写 Pi `Π`](#capital-pi) - *products of sequences*
- [管道 `||`](#pipes)
  - [绝对值](#absolute-value)
  - [取模](#euclidean-norm)
  - [行列式](#determinant)
- [帽子 **`â`**](#hat) - *unit vector*
- ["属于" `∈` `∉`](#element)
- [常见数字集 `ℝ` `ℤ` `ℚ` `ℕ`](#common-number-sets)
- [function `ƒ`](#function)
  - [piecewise function](#piecewise-function)
  - [common functions](#common-functions)
  - [function notation `↦` `→`](#function-notation)
- [prime `′`](#prime)
- [floor & ceiling `⌊` `⌉`](#floor--ceiling)
- [arrows](#arrows)
  - [material implication `⇒` `→`](#material-implication)
  - [equality `<` `≥` `≫`](#equality)
  - [conjunction & disjunction `∧` `∨`](#conjunction--disjunction)
- [logical negation `¬` `~` `!`](#logical-negation)
- [intervals](#intervals)
- [more...](#more)

## 变量名惯例

有很多命名惯例取决于上下文和所学领域，他们并不太一致。然而在一些文献中你会发现变量名遵循一些模式，例如：

- *s* - 斜体小写字母用做标量 （例如一个数字）
- **x** - 粗体小写字母用做向量 （例如一个2D点）
- **A** - 粗体大写字母用做矩阵 （例如一个3D变换）
- *θ* - 斜体小写希腊字母用做常量和特殊变量 （例如 [欧拉角 *θ*, *theta*](https://en.wikipedia.org/wiki/Spherical_coordinate_system))

本指南也基于这个格式。

## 等号

有很多符号很像等号，常见的如：

- `=` 用做相等 （值相同）
- `≠` 表示不相等 （值不同）
- `≈` 表示约等于 （`π ≈ 3.14159`）
- `:=` 表示定义 （A 被定义为 B）

在 JavaScript中:

```js
// 相等
2 === 3

// 不相等
2 !== 3

// 约等于
almostEqual(Math.PI, 3.14159, 1e-5)

function almostEqual(a, b, epsilon) {
  return Math.abs(a - b) <= epsilon
}
```

你也许看过 `:=`， `=:` 和 `=` 符号用来表示 *定义*。<sup>[1]</sup>

例如，下边定义 *x* 为 2*kj*。

![equals1](http://latex.codecogs.com/svg.latex?x%20%3A%3D%202kj)

<!-- x := 2kj -->

在JavaScript中，我们用`var`来 *定义* 变量和提供别名：

```js
var x = 2 * k * j
```

然而，这里的x值是可变的，仅是当时的一个快照。在某些有预处理器语言中的`#define`语句才比较接近于数学中的 *定义*。

在JavaScript (ES6)中，更精确的 *定义* ，应该有点类似这样：

```js
const f = (k, j) => 2 * k * j
```

与此不同的是，下边这句表示的是相等：

![equals2](http://latex.codecogs.com/svg.latex?x%20%3D%202kj)

<!-- x = 2kj -->

上边的等式也可以解释为一个 [断言](https://developer.mozilla.org/en-US/docs/Web/API/console/assert):

```js
console.assert(x === (2 * k * j))
```


## 平方根与复数

一个平方根运算是这种形式:

![squareroot](http://latex.codecogs.com/svg.latex?%5Cleft%28%5Csqrt%7Bx%7D%5Cright%29%5E2%20%3D%20x)

<!-- \left(\sqrt{x}\right)^2 = x -->

在编程语言中我们使用`sqrt` 函数， 例如： 

```js
var x = 9;
console.log(Math.sqrt(x));
//=> 3
```

复数是表达式 ![complex](http://latex.codecogs.com/svg.latex?a&space;&plus;&space;ib) 的形式， 其中 ![a](http://latex.codecogs.com/svg.latex?a) 是实数部分， ![b](http://latex.codecogs.com/svg.latex?b) 是虚数部分。 虚数 ![i](http://latex.codecogs.com/svg.latex?i) 的定义为：

![imaginary](http://latex.codecogs.com/svg.latex?i%3D%5Csqrt%7B-1%7D).
<!-- i=\sqrt{-1} -->

JavaScript没有内置复数的功能，但有一些库支持复数算法。例如 [mathjs](https://www.npmjs.com/package/mathjs):

```js
var math = require('mathjs')

var a = math.complex(3, -1)
//=> { re: 3, im: -1 }

var b = math.sqrt(-1)
//=> { re: 0, im: -1 }

console.log(math.multiply(a, b).toString())
//=> '1 + 3i'
```

这个库还支持字符串表达式求值， 所以上边的可以写为：

```js
console.log(math.eval('(3 - i) * i').toString())
//=> '1 + 3i'
```

其他实现:

- [immutable-complex](https://www.npmjs.com/package/immutable-complex)
- [complex-js](https://www.npmjs.com/package/complex-js)
- [Numeric-js](http://www.numericjs.com/)

## 点 & 叉

点 `·` 和叉 `×` 符号有些不同，这取决于上下文。

虽然看上去很明显，但在进入下一部分之前，理解他们之间微妙的不同是非常重要的。

#### 标量乘法

两个符号都可以表示简单的标量之间的乘法。下边的写法意思相同：

![dotcross1](http://latex.codecogs.com/svg.latex?5%20%5Ccdot%204%20%3D%205%20%5Ctimes%204)

<!-- 5 \cdot 4 = 5 \times 4 -->

在编程语言中，我们倾向用星号表示相乘：

```js
var result = 5 * 4
```

通常，乘法符号只是为了避免意义模糊（例如两个数字之间的）。这里，我们可以完全省略：

![dotcross2](http://latex.codecogs.com/svg.latex?3kj)

<!-- 3kj -->

如果这些变量表示的是标量，则代码应该这样写:

```js
var result = 3 * k * j
```

#### 向量乘法

表示向量和标量之间相乘，或两向量的元素积（element-wise multiplication），我们不用点 `·` 或叉 `×` 符号。 这些符号在线性代数中有不同的意思，后边讨论。

让我们用之前的例子，但用在向量上。对于向量的元素积（element-wise vector multiplication）来说，你可能会看到用一个空心点来表示[Hadamard product](https://en.wikipedia.org/wiki/Hadamard_product_%28matrices%29).<sup>[2]</sup>

![dotcross3](http://latex.codecogs.com/svg.latex?3%5Cmathbf%7Bk%7D%5Ccirc%5Cmathbf%7Bj%7D)

<!-- 3\mathbf{k}\circ\mathbf{j} -->

某些时候作者会定义个不同的符号，例如圆心点 `⊙` 或实心圈 `●` 。<sup>[3]</sup>

这是对应的代码，使用数组`[x, y]`来表示2D向量。

```js
var s = 3
var k = [ 1, 2 ]
var j = [ 2, 3 ]

var tmp = multiply(k, j)
var result = multiplyScalar(tmp, s)
//=> [ 6, 18 ]
```

`multiply` 和 `multiplyScalar` 函数应该这样:

```js
function multiply(a, b) {
  return [ a[0] * b[0], a[1] * b[1] ]
}

function multiplyScalar(a, scalar) {
  return [ a[0] * scalar, a[1] * scalar ]
}
```

同样的，矩阵相乘也不用 `·` 或 `×` 符号。 矩阵乘法会在后边章节提到.

#### 点乘

点符号 `·` 可用来表示两向量之间的 [*点乘*](https://en.wikipedia.org/wiki/Dot_product) 。 由于其值是一个标量，通常被叫做*标量积（scalar product）*。

![dotcross4](http://latex.codecogs.com/svg.latex?%5Cmathbf%7Bk%7D%5Ccdot%20%5Cmathbf%7Bj%7D)

<!-- \mathbf{k}\cdot \mathbf{j} -->

这在线性代数和3D向量中是非常常见的，代码类似这样：

```js
var k = [ 0, 1, 0 ]
var j = [ 1, 0, 0 ]

var d = dot(k, j)
//=> 0
```

结果为 `0` 告诉我们两向量互相垂直. 这是3元素向量的点乘函数:

```js
function dot(a, b) {
  return a[0] * b[0] + a[1] * b[1] + a[2] * b[2]
}
```

#### 叉乘

`×`符号可以用来表示两向量的 [*叉乘*](https://en.wikipedia.org/wiki/Cross_product)。

![dotcross5](http://latex.codecogs.com/svg.latex?%5Cmathbf%7Bk%7D%5Ctimes%20%5Cmathbf%7Bj%7D)

<!-- \mathbf{k}\times \mathbf{j} -->

在代码中，应该是这样:

```js
var k = [ 0, 1, 0 ]
var j = [ 1, 0, 0 ]

var result = cross(k, j)
//=> [ 0, 0, -1 ]
```

这里得到结果为 `[ 0, 0, -1 ]`，这个向量既垂直于 **k** 也垂直于 **j**。

叉乘`cross` 函数：

```js
function cross(a, b) {
  var ax = a[0], ay = a[1], az = a[2],
    bx = b[0], by = b[1], bz = b[2]

  var rx = ay * bz - az * by
  var ry = az * bx - ax * bz
  var rz = ax * by - ay * bx
  return [ rx, ry, rz ]
}
```

其他向量乘法，叉乘，点乘的实现：

- [gl-vec3](https://github.com/stackgl/gl-vec3)
- [gl-vec2](https://github.com/stackgl/gl-vec2)
- [vectors](https://github.com/hughsk/vectors) - includes n-dimensional

## sigma 

大写希腊字母 `Σ` (Sigma) 用来表示 [总和 Summation](https://en.wikipedia.org/wiki/Summation)。 换句话说就是对一些数字求和。

![sigma](http://latex.codecogs.com/svg.latex?%5Csum_%7Bi%3D1%7D%5E%7B100%7Di)

<!-- \sum_{i=1}^{100}i -->

这里, `i=1` 是说从`1`开始一直到Sigma上边的数字`100`为止。这些分别为上下边界。 "E"右边的 *i* 告诉我们是在求和。代码：

```js
var sum = 0
for (var i = 1; i <= 100; i++) {
  sum += i
}
```

`sum`的结果为`5050`.

**提示：** 对于整数， 这个特殊形式可以优化为：

```js
var n = 100 // upper bound
var sum = (n * (n + 1)) / 2
```

这里有另一个例子，这里的*i*，或“想要求和的”是不同的:

![sum2](http://latex.codecogs.com/svg.latex?%5Csum_%7Bi%3D1%7D%5E%7B100%7D%282i&plus;1%29)

<!-- \sum_{i=1}^{100}(2i+1) -->

代码：

```js
var sum = 0
for (var i = 1; i <= 100; i++) {
  sum += (2 * i + 1)
}
```

结果 `sum` 为 `10200`.

符号可被嵌套，非常像嵌套一个`for` 循环。 你应该先求和最右边的 sigma ， 除非作者加入了括号。然而下边的例子，由于我们处理有限的和，顺序就不重要了。

![sigma3](http://latex.codecogs.com/svg.latex?%5Csum_%7Bi%3D1%7D%5E%7B2%7D%5Csum_%7Bj%3D4%7D%5E%7B6%7D%283ij%29)

<!-- \sum_{i=1}^{2}\sum_{j=4}^{6}(3ij) -->

代码：

```js
var sum = 0
for (var i = 1; i <= 2; i++) {
  for (var j = 4; j <= 6; j++) {
    sum += (3 * i * j)
  }
}
```

这里，`sum` 值为 `135`。

## 大写 Pi

大写 Pi 或 "大 Pi" 与 [Sigma](#sigma) 非常接近， 不同的是我们用乘法取得一系列数字的乘积。 

看下边:

![capitalPi](http://latex.codecogs.com/svg.latex?%5Cprod_%7Bi%3D1%7D%5E%7B6%7Di)

<!-- \prod_{i=1}^{6}i -->

代码应该类似这样：

```js
var value = 1
for (var i = 1; i <= 6; i++) {
  value *= i
}
```

`value` 结果应得到 `720`.

## 管道（pipes）

管道符号，就是*竖条（bars）*，根据上下文不同，可以表示不同意思。下边的是3种常见用途[绝对值](#absolute-value), [取模](#euclidean-norm), 和 [行列式](#determinant)。

这3种特性都是描述对象的 *长度（length）* 。

#### 绝对值

![pipes1](http://latex.codecogs.com/svg.latex?%5Cleft%20%7C%20x%20%5Cright%20%7C)

<!-- \left | x \right | -->

对于数字 *x*, `|x|` 表示 *x* 的绝对值。代码为：

```js
var x = -5
var result = Math.abs(x)
// => 5
```

#### 取模（Euclidean norm）

![pipes4](http://latex.codecogs.com/svg.latex?%5Cleft%20%5C%7C%20%5Cmathbf%7Bv%7D%20%5Cright%20%5C%7C)

<!-- \left \| \mathbf{v} \right \| -->

对于向量 **v**， `‖v‖` 是 **v** 的[取模（Euclidean norm）](https://en.wikipedia.org/wiki/Norm_%28mathematics%29#Euclidean_norm) 。也叫做向量的 "量级（magnitude）" 或 "长度（length）" 。

通常用双竖线来避免与*绝对值*符号混淆，但有些时候也会看见单竖线。

![pipes2](http://latex.codecogs.com/svg.latex?%5Cleft%20%7C%20%5Cmathbf%7Bv%7D%20%5Cright%20%7C)

<!-- \left | \mathbf{v} \right | -->

这里的例子用数组 `[x, y, z]` 来表示一个3D向量。

```js
var v = [ 0, 4, -3 ]
length(v)
//=> 5
```

`length`函数：

```js
function length (vec) {
  var x = vec[0]
  var y = vec[1]
  var z = vec[2]
  return Math.sqrt(x * x + y * y + z * z)
}
```

其他实现：

- [magnitude](https://github.com/mattdesl/magnitude/blob/864ff5a7eb763d34bf154ac5f5332d7601192b70/index.js) - n-dimensional
- [gl-vec2/length](https://github.com/stackgl/gl-vec2/blob/21f460a371540258521fd2f720d80f14e87bd400/length.js) - 2D vector
- [gl-vec3/length](https://github.com/stackgl/gl-vec3/blob/507480fa57ba7c5fb70679cf531175a52c48cf53/length.js) - 3D vector

#### 行列式

![pipes3](http://latex.codecogs.com/svg.latex?%5Cleft%20%7C%5Cmathbf%7BA%7D%20%5Cright%20%7C)

<!-- \left |\mathbf{A}  \right | -->

对于一个矩阵 **A**, `|A|` 表示矩阵 **A** 的[行列式（determinant）](https://en.wikipedia.org/wiki/Determinant)。

这是一个计算2x2矩阵行列式的粒子，矩阵用一个column-major格式的浮点数数组表示。

```js
var determinant = require('gl-mat2/determinant')

var matrix = [ 1, 0, 0, 1 ]
var det = determinant(matrix)
//=> 1
```

实现：

- [gl-mat4/determinant](https://github.com/stackgl/gl-mat4/blob/c2e2de728fe7eba592f74cd02266100cc21ec89a/determinant.js) - also see [gl-mat3](https://github.com/stackgl/gl-mat3) and [gl-mat2](https://github.com/stackgl/gl-mat2)
- [ndarray-determinant](https://www.npmjs.com/package/ndarray-determinant)
- [glsl-determinant](https://www.npmjs.com/package/glsl-determinant)
- [robust-determinant](https://www.npmjs.com/package/robust-determinant)
- [robust-determinant-2](https://www.npmjs.com/package/robust-determinant-2) and [robust-determinant-3](https://www.npmjs.com/package/robust-determinant-3), specifically for 2x2 and 3x3 matrices, respectively

## 帽子

在几何里，字母上的“帽子”符号用来表示一个[单位向量](https://en.wikipedia.org/wiki/Unit_vector)。例如，这是向量 **a** 的单位向量。

![hat](http://latex.codecogs.com/svg.latex?%5Chat%7B%5Cmathbf%7Ba%7D%7D)

<!-- \hat{\mathbf{a}} -->

在笛卡尔空间中，单位向量的长度为1。意思是向量的每个部分都在-1.0 到 1.0之间。这里我们 *归一化（normalize）* 一个3D向量为单位向量。

```js
var a = [ 0, 4, -3 ]
normalize(a)
//=> [ 0, 0.8, -0.6 ]
```

这是`归一化（normalize）` 函数，接收一个3D向量参数：

```js
function normalize(vec) {
  var x = vec[0]
  var y = vec[1]
  var z = vec[2]
  var squaredLength = x * x + y * y + z * z

  if (squaredLength > 0) {
    var length = Math.sqrt(squaredLength)
    vec[0] = vec[0] / length
    vec[1] = vec[1] / length
    vec[2] = vec[2] / length
  }
  return vec
}
```

其他实现：

- [gl-vec3/normalize](https://github.com/stackgl/gl-vec3/blob/507480fa57ba7c5fb70679cf531175a52c48cf53/normalize.js) and [gl-vec2/normalize](https://github.com/stackgl/gl-vec2/blob/21f460a371540258521fd2f720d80f14e87bd400/normalize.js)
- [vectors/normalize-nd](https://github.com/hughsk/vectors/blob/master/normalize-nd.js) (n-dimensional)

## 属于

集合理论中，“属于”符号 `∈` 和 `∋` 可以被用来描述某物是否为集合中的一个元素。例如：

![element1](http://latex.codecogs.com/svg.latex?A%3D%5Cleft%20%5C%7B3%2C9%2C14%7D%7B%20%5Cright%20%5C%7D%2C%203%20%5Cin%20A)

<!-- A=\left \{3,9,14}{  \right \}, 3 \in A -->

这里我们有一个数字集 *A* `{ 3, 9, 14 }` 而且我们说 `3` 是“属于”这个集合的。 

在ES5种一个简单的实现应该这样：

```js
var A = [ 3, 9, 14 ]

A.indexOf(3) >= 0
//=> true
```

然而，可以用只能保存唯一值的`Set`，这样更精确。这是ES6的一个特性。

```js
var A = new Set([ 3, 9, 14 ])

A.has(3)
//=> true
```

反向的 `∋` 意义相同，只是顺序改变：

![element2](http://latex.codecogs.com/svg.latex?A%3D%5Cleft%20%5C%7B3%2C9%2C14%7D%7B%20%5Cright%20%5C%7D%2C%20A%20%5Cni%203)

<!-- A=\left \{3,9,14}{  \right \}, A \ni 3 -->

你可以使用 "不属于" 符号 `∉` 和 `∌` 例如：

![element3](http://latex.codecogs.com/svg.latex?A%3D%5Cleft%20%5C%7B3%2C9%2C14%7D%7B%20%5Cright%20%5C%7D%2C%206%20%5Cnotin%20A)

<!-- A=\left \{3,9,14}{  \right \}, 6 \notin A -->

## 常见数字集

你可能在一些公式中看见一些大[黑板粗体字](https://en.wikipedia.org/wiki/Blackboard_bold)。他们一般是用来描述集合的。

例如，我们可以描述 *k* 是[属于](#element) `ℝ`集中的一个元素。

![real](http://latex.codecogs.com/svg.latex?k%20%5Cin%20%5Cmathbb%7BR%7D)

<!-- k \in \mathbb{R} -->

下边列出一些常见集和他们的符号。

#### `ℝ` 实数（real numbers）

大 `ℝ` 描述 *实数（real numbers）* 的集合。他们包括整数，有理数，无理数。

JavaScript认为整数和浮点数为相同类型，所以下边将是一个*k* ∈ ℝ 的简单测试：

```js
function isReal (k) {
  return typeof k === 'number' && isFinite(k);
}
```

*注意：* 实数也是 *有限数（finite）*，*非无限的（not infinite）*

#### `ℚ` 有理数（rational numbers）

Rational numbers are real numbers that can be expressed as a fraction, or *ratio* (like `⅗`). Rational numbers cannot have zero as a denominator.

This also means that all integers are rational numbers, since the denominator can be expressed as 1.

An irrational number, on the other hand, is one that cannot be expressed as a ratio, like π (PI). 

#### `ℤ` 整数（integers）

An integer, i.e. a real number that has no fractional part. These can be positive or negative.

A simple test in JavaScript might look like this:

```js
function isInteger (n) {
  return typeof n === 'number' && n % 1 === 0
}
```

#### `ℕ` 自然数（natural numbers）

A natural number, a positive and non-negative integer. Depending on the context and field of study, the set may or may not include zero, so it could look like either of these:

```js
{ 0, 1, 2, 3, ... }
{ 1, 2, 3, 4, ... }
```

The latter is more common in computer science, for example:

```js
function isNaturalNumber (n) {
  return isInteger(n) && n >= 0
}
```

#### `ℂ` complex numbers

A complex number is a combination of a real and imaginary number, viewed as a co-ordinate in the 2D plane. For more info, see [A Visual, Intuitive Guide to Imaginary Numbers](http://betterexplained.com/articles/a-visual-intuitive-guide-to-imaginary-numbers/).

## function

[Functions](https://en.wikipedia.org/wiki/Function_%28mathematics%29) are fundamental features of mathematics, and the concept is fairly easy to translate into code.

A function relates an input to an output value. For example, the following is a function:

![function1](http://latex.codecogs.com/svg.latex?x%5E%7B2%7D)

<!-- x^{2} -->

We can give this function a *name*. Commonly, we use `ƒ` to describe a function, but it could be named `A(x)` or anything else.

![function2](http://latex.codecogs.com/svg.latex?f%5Cleft%20%28x%20%5Cright%20%29%20%3D%20x%5E%7B2%7D)

<!-- f\left (x  \right ) = x^{2} -->

In code, we might name it `square` and write it like this:

```js
function square (x) {
  return Math.pow(x, 2)
}
```

Sometimes a function is not named, and instead the output is written.

![function3](http://latex.codecogs.com/svg.latex?y%20%3D%20x%5E%7B2%7D)

<!-- y = x^{2} -->

In the above example, *x* is the input, the relationship is *squaring*, and *y* is the output.

Functions can also have multiple parameters, like in a programming language. These are known as *arguments* in mathematics, and the number of arguments a function takes is known as the *arity* of the function.

![function4](http://latex.codecogs.com/svg.latex?f%28x%2Cy%29%20%3D%20%5Csqrt%7Bx%5E2%20&plus;%20y%5E2%7D)

<!-- f(x,y) = \sqrt{x^2 + y^2} -->

In code:

```js
function length (x, y) {
  return Math.sqrt(x * x + y * y)
}
```

### piecewise function

Some functions will use different relationships depending on the input value, *x*.

The following function *ƒ* chooses between two "sub functions" depending on the input value.

![piecewise1](http://latex.codecogs.com/svg.latex?f%28x%29%3D%20%5Cbegin%7Bcases%7D%20%5Cfrac%7Bx%5E2-x%7D%7Bx%7D%2C%26%20%5Ctext%7Bif%20%7D%20x%5Cgeq%201%5C%5C%200%2C%20%26%20%5Ctext%7Botherwise%7D%20%5Cend%7Bcases%7D)

<!--    f(x)= 
\begin{cases}
    \frac{x^2-x}{x},& \text{if } x\geq 1\\
    0, & \text{otherwise}
\end{cases} -->

This is very similar to `if` / `else` in code. The right-side conditions are often written as **"for x < 0"** or **"if x = 0"**. If the condition is true, the function to the left is used.

In piecewise functions, **"otherwise"** and **"elsewhere"** are analogous to the `else` statement in code.

```js
function f (x) {
  if (x >= 1) {
    return (Math.pow(x, 2) - x) / x
  } else {
    return 0
  }
}
```

### common functions

There are some function names that are ubiquitous in mathematics. For a programmer, these might be analogous to functions "built-in" to the language (like `parseInt` in JavaScript).

One such example is the *sgn* function. This is the *signum* or *sign* function. Let's use [piecewise function](#piecewise-function) notation to describe it:

![sgn](http://latex.codecogs.com/svg.latex?sgn%28x%29%20%3A%3D%20%5Cbegin%7Bcases%7D%20-1%26%20%5Ctext%7Bif%20%7D%20x%20%3C%200%5C%5C%200%2C%20%26%20%5Ctext%7Bif%20%7D%20%7Bx%20%3D%200%7D%5C%5C%201%2C%20%26%20%5Ctext%7Bif%20%7D%20x%20%3E%200%5C%5C%20%5Cend%7Bcases%7D)

<!-- sgn(x) := 
\begin{cases}
    -1& \text{if } x < 0\\
    0, & \text{if } {x = 0}\\
    1, & \text{if } x > 0\\
\end{cases} -->

In code, it might look like this:

```js
function sgn (x) {
  if (x < 0) return -1
  if (x > 0) return 1
  return 0
}
```

See [signum](https://github.com/scijs/signum) for this function as a module.

Other examples of such functions: *sin*, *cos*, *tan*.

### function notation

In some literature, functions may be defined with more explicit notation. For example, let's go back to the `square` function we mentioned earlier:

![function2](http://latex.codecogs.com/svg.latex?f%5Cleft%20%28x%20%5Cright%20%29%20%3D%20x%5E%7B2%7D)

<!-- f\left (x  \right ) = x^{2} -->

It might also be written in the following form:

![mapsto](http://latex.codecogs.com/svg.latex?f%20%3A%20x%20%5Cmapsto%20x%5E2)

<!-- f : x \mapsto x^2 -->

The arrow here with a tail typically means "maps to," as in *x maps to x<sup>2</sup>*. 

Sometimes, when it isn't obvious, the notation will also describe the *domain* and *codomain* of the function. A more formal definition of *ƒ* might be written as:

![funcnot](http://latex.codecogs.com/svg.latex?%5Cbegin%7Balign*%7D%20f%20%3A%26%5Cmathbb%7BR%7D%20%5Crightarrow%20%5Cmathbb%7BR%7D%5C%5C%20%26x%20%5Cmapsto%20x%5E2%20%5Cend%7Balign*%7D)

<!-- \begin{align*}
f :&\mathbb{R} \rightarrow \mathbb{R}\\
&x \mapsto x^2 
\end{align*}
 -->

A function's *domain* and *codomain* is a bit like its *input* and *output* types, respectively. Here's another example, using our earlier *sgn* function, which outputs an integer:

![domain2](http://latex.codecogs.com/svg.latex?sgn%20%3A%20%5Cmathbb%7BR%7D%20%5Crightarrow%20%5Cmathbb%7BZ%7D)

<!-- sgn : \mathbb{R} \rightarrow \mathbb{Z} -->

The arrow here (without a tail) is used to map one *set* to another.

In JavaScript and other dynamically typed languages, you might use documentation and/or runtime checks to explain and validate a function's input/output. Example:

```js
/**
 * Squares a number.
 * @param  {Number} a real number
 * @return {Number} a real number
 */
function square (a) {
  if (typeof a !== 'number') {
    throw new TypeError('expected a number')
  }
  return Math.pow(a, 2)
}
```

Some tools like [flowtype](http://flowtype.org/) attempt to bring static typing into JavaScript.

Other languages, like Java, allow for true method overloading based on the static types of a function's input/output. This is closer to mathematics: two functions are not the same if they use a different *domain*.

## prime

The prime symbol (`′`) is often used in variable names to describe things which are similar, without giving it a different name altogether. It can describe the "next value" after some transformation.

For example, if we take a 2D point *(x, y)* and rotate it, you might name the result *(x′, y′)*. Or, the *transpose* of matrix **M** might be named **M′**.

In code, we typically just assign the variable a more descriptive name, like `transformedPosition`.

For a mathematical [function](#function), the prime symbol often describes the *derivative* of that function. Derivatives will be explained in a future section. Let's take our earlier function:

![function2](http://latex.codecogs.com/svg.latex?f%5Cleft%20%28x%20%5Cright%20%29%20%3D%20x%5E%7B2%7D)

<!-- f\left (x  \right ) = x^{2} -->

Its derivative could be written with a prime `′` symbol:

![prime1](http://latex.codecogs.com/svg.latex?f%27%28x%29%20%3D%202x)

<!-- f'(x) = 2x -->

In code:

```js
function f (x) {
  return Math.pow(x, 2)
}

function fPrime (x) {
  return 2 * x
}
```

Multiple prime symbols can be used to describe the second derivative *ƒ′′* and third derivative *ƒ′′′*. After this, authors typically express higher orders with roman numerals *ƒ*<sup>IV</sup> or superscript numbers *ƒ*<sup>(n)</sup>.

## floor & ceiling

The special brackets `⌊x⌋` and `⌈x⌉` represent the *floor* and *ceil* functions, respectively.

![floor](http://latex.codecogs.com/svg.latex?floor%28x%29%20%3D%20%5Clfloor%20x%20%5Crfloor)

<!-- floor(x) =  \lfloor x \rfloor -->

![ceil](http://latex.codecogs.com/svg.latex?ceil%28x%29%20%3D%20%5Clceil%20x%20%5Crceil)

<!-- ceil(x) =  \lceil x \rceil -->

In code:

```js
Math.floor(x)
Math.ceil(x)
```

When the two symbols are mixed `⌊x⌉`, it typically represents a function that rounds to the nearest integer:

![round](http://latex.codecogs.com/svg.latex?round%28x%29%20%3D%20%5Clfloor%20x%20%5Crceil)

<!-- round(x) =  \lfloor x \rceil -->

In code:

```js
Math.round(x)
```

## arrows

Arrows are often used in [function notation](#function-notation). Here are a few other areas you might see them.

#### material implication

Arrows like `⇒` and `→` are sometimes used in logic for *material implication.* That is, if A is true, then B is also true.

![material1](http://latex.codecogs.com/svg.latex?A%20%5CRightarrow%20B)

<!-- A \Rightarrow B -->

Interpreting this as code might look like this:

```js
if (A === true) {
  console.assert(B === true)
}
```

The arrows can go in either direction `⇐` `⇒`, or both `⇔`. When *A ⇒ B* and *B ⇒ A*, they are said to be equivalent:

![material-equiv](http://latex.codecogs.com/svg.latex?A%20%5CLeftrightarrow%20B)

<!-- A \Leftrightarrow B -->

#### equality

In math, the `<` `>` `≤` and `≥` are typically used in the same way we use them in code: *less than*, *greater than*, *less than or equal to* and *greater than or equal to*, respectively.

```js
50 > 2 === true
2 < 10 === true
3 <= 4 === true
4 >= 4 === true
```

On rare occasions you might see a slash through these symbols, to describe *not*. As in, *k* is "not greater than" *j*.

![ngt](http://latex.codecogs.com/svg.latex?k%20%5Cngtr%20j)

<!-- k \ngtr j -->

The `≪` and `≫` are sometimes used to represent *significant* inequality. That is, *k* is an [order of magnitude](https://en.wikipedia.org/wiki/Order_of_magnitude) larger than *j*.

![orderofmag](http://latex.codecogs.com/svg.latex?k%20%5Cgg%20j)

<!-- k \gg j -->

In mathematics, *order of magnitude* is rather specific; it is not just a "really big difference." A simple example of the above:

```js
orderOfMagnitude(k) > orderOfMagnitude(j)
```

And below is our `orderOfMagnitude` function, using [Math.trunc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc) (ES6).

```js
function log10(n) {
  // logarithm in base 10
  return Math.log(n) / Math.LN10
}

function orderOfMagnitude (n) {
  return Math.trunc(log10(n))
}
```

<sup>*Note:* This is not numerically robust.</sup>

See [math-trunc](https://www.npmjs.com/package/math-trunc) for a ponyfill in ES5.

#### conjunction & disjunction

Another use of arrows in logic is conjunction `∧` and disjunction `∨`. They are analogous to a programmer's `AND` and `OR` operators, respectively.

The following shows conjunction `∧`, the logical `AND`.

![and](http://latex.codecogs.com/svg.latex?k%20%3E%202%20%5Cland%20k%20%3C%204%20%5CLeftrightarrow%20k%20%3D%203)

<!-- k > 2 \land k <  4 \Leftrightarrow k = 3   -->

In JavaScript, we use `&&`. Assuming *k* is a natural number, the logic implies that *k* is 3:

```js
if (k > 2 && k < 4) {
  console.assert(k === 3)
}
```

Since both sides are equivalent `⇔`, it also implies the following:

```js
if (k === 3) {
  console.assert(k > 2 && k < 4)
}
```

The down arrow `∨` is logical disjunction, like the OR operator.

![logic-or](http://latex.codecogs.com/svg.latex?A%20%5Clor%20B)

<!-- A \lor B -->

In code:

```js
A || B
```

## logical negation

Occasionally, the `¬`, `~` and `!` symbols are used to represent logical `NOT`. For example, *¬A* is only true if A is false.

Here is a simple example using the *not* symbol:

![negation](http://latex.codecogs.com/svg.latex?x%20%5Cneq%20y%20%5CLeftrightarrow%20%5Clnot%28x%20%3D%20y%29)

<!-- x \neq y \Leftrightarrow \lnot(x = y) -->

An example of how we might interpret this in code:

```js
if (x !== y) {
  console.assert(!(x === y))
}
```

*Note:* The tilde `~` has many different meanings depending on context. For example, *row equivalence* (matrix theory) or *same order of magnitude* (discussed in [equality](#equality)).

## intervals

Sometimes a function deals with real numbers restricted to some range of values, such a constraint can be represented using an *interval*

For example we can represent the numbers between zero and one including/not including zero and/or one as:

- Not including zero or one: ![interval-opened-left-opened-right](http://latex.codecogs.com/svg.latex?%280%2C%201%29)

<!-- (0, 1) -->

- Including zero or but not one: ![interval-closed-left-opened-right](http://latex.codecogs.com/svg.latex?%5B0%2C%201%29)

<!-- [0, 1) -->

- Not including zero but including one: ![interval-opened-left-closed-right](http://latex.codecogs.com/svg.latex?%280%2C%201%5D)

<!-- (0, 1] -->

- Including zero and one: ![interval-closed-left-closed-right](http://latex.codecogs.com/svg.latex?%5B0%2C%201%5D)

<!-- [0, 1] -->

For example we to indicate that a point `x` is in the unit cube in 3D we say:

![interval-unit-cube](http://latex.codecogs.com/svg.latex?x%20%5Cin%20%5B0%2C%201%5D%5E3)

<!-- x \in [0, 1]^3 -->

In code we can represent an interval using a two element 1d array:

```js
var nextafter = require('nextafter')

var a = [nextafter(0, Infinity), nextafter(1, -Infinity)]     // open interval
var b = [nextafter(0, Infinity), 1]                           // interval closed on the left 
var c = [0, nextafter(1, -Infinity)]                          // interval closed on the right
var d = [0, 1]                                                // closed interval
```

Intervals are used in conjunction with set operations:

- *intersection* e.g. ![interval-intersection](http://latex.codecogs.com/svg.latex?%5B3%2C%205%29%20%5Ccap%20%5B4%2C%206%5D%20%3D%20%5B4%2C%205%29)

<!-- [3, 5) \cap [4, 6] = [4, 5) -->

- *union* e.g. ![interval-union](http://latex.codecogs.com/svg.latex?%5B3%2C%205%29%20%5Ccup%20%5B4%2C%206%5D%20%3D%20%5B3%2C%206%5D)

<!-- [3, 5) \cup [4, 6] = [3, 6] -->

- *difference* e.g. ![interval-difference-1](http://latex.codecogs.com/svg.latex?%5B3%2C%205%29%20-%20%5B4%2C%206%5D%20%3D%20%5B3%2C%204%29) and ![interval-difference-2](http://latex.codecogs.com/svg.latex?%5B4%2C%206%5D%20-%20%5B3%2C%205%29%20%3D%20%5B5%2C%206%5D)

<!-- [3, 5) - [4, 6] = [3, 4) -->
<!-- [4, 6] - [3, 5)  = [5, 6] -->

In code:

```js
var Interval = require('interval-arithmetic')
var nexafter = require('nextafter')

var a = Interval(3, nexafter(5, -Infinity))
var b = Interval(4, 6)

Interval.intersection(a, b)
// {lo: 4, hi: 4.999999999999999}

Interval.union(a, b)
// {lo: 3, hi: 6}

Interval.difference(a, b)
// {lo: 3, hi: 3.9999999999999996}

Interval.difference(b, a)
// {lo: 5, hi: 6}
```

See:

- [next-after](https://github.com/scijs/nextafter) 
- [interval-arithmetic](https://github.com/maurizzzio/interval-arithmetic)

## more...

Like this guide? Suggest some [more features](https://github.com/Jam3/math-as-code/issues/1) or send us a Pull Request!

## Contributing

For details on how to contribute, see [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

MIT, see [LICENSE.md](http://github.com/Jam3/math-as-code/blob/master/LICENSE.md) for details.

[1]: http://mimosa-pudica.net/improved-oren-nayar.html#images
[2]: http://buzzard.ups.edu/courses/2007spring/projects/million-paper.pdf
[3]: https://www.math.washington.edu/~morrow/464_12/fft.pdf