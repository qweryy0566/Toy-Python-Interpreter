# Python Interpreter

a toy python interpreter

## 具体要求

### 程序结构

从上到下逐行执行。

### 文法规则

ASCII 编码，区分大小写。中文字符是未定义的。

### 关键字

```python
None True False def return break continue if elif else while or and not
```

关键字不可作为变量名或函数名。

### 空白字符的处理

空格、制表符在源文件中可以区分词素 (Token)，同时在一行的开头可以表示缩进。

换行符表示新的语句的开始。

### 注释

从 `#` 开始到本行结束的内容都会被作为注释。

### 标识符

标识符的第一个字符必须是英文字母，第二个字符开始可以是英文字母、数字或者下划线。

标识符区分大小写。长度超过 $64$ 个字符的标识符是未定义的。

### 常量

#### 逻辑常量

`True` 为真，`False` 为假。

#### 整数常量

整数常量以十进制表示。**整数常量不设负数**，负数可以由正数取负号得到。

整数的范围是没有限制的，这意味着你必须实现高精度整数。

首位为 $0$ 的整数常量是未定义的。

#### 字符串常量

字符串常量是由双引号或单引号括起来的字符串。

可以由两个字符串拼接而形成新的字符串常量。如 `"123""456"` 相当于 `"123456"`。


字符串中的所有字符必须是可示字符 (printable character) 或空格中的一种。

#### 空值常量

`None` 用来表示变量没有指向任何值。

### 运算符

#### 算术运算符

```python
+ - * / // %
```

在字符串意义下 `+` 表示拼接，`* a` 表示拼接 $a$ 次。

`/` 是浮点除，`//` 是整除，`%` 是模运算。

运算的隐式类型转换参考 c++ 语法。当整除与模涉及到浮点数，此行为是未定义的行为。

整除（无论正负）可以看成浮点除之后向下取整（当然这不是正确写法，因为这样会造成有效数字丢失）

模运算无论正负定义为： `a%b=a-(a//b)*b`。

#### 关系运算符

```python
< > <= >= == !=
```

连续比较：若出现 `1<2>3` 这样连续的关系运算符，将它拆成相邻的比较并用 `and` 连接，但**每个值只计算一遍**。如 `a()<b()<c()`，相当于判断 `a()<b() and b()<c()` 但是 `a(),b(),c()` 只调用一遍。

#### 逻辑运算符

```python
and or not
```

**与标准 python 语法不同，这里只返回布尔类型**。

请注意逻辑运算的短路。

#### 赋值运算符

```python
=
```

给一个变量赋值的意义是将这个变量指向右值，右值不管类型。

对于一个之前未定义的变量，赋值运算会先定义变量。

**与标准 python 不同全局变量的生效范围是全部范围 （不用 global 关键字即可访问）。**

局部变量的生效范围是在当前语句块（被缩进和取消缩进包起来的部分），具体局部变量和全局变量划分规则和 c++ 一样。

如 `a=1,a=”123”,a=1.1` 这三条语句依次执行，再输出 `a`，结果是 `1.1`。

可以连等，如 `a=b="123"` 意思是 `b="123",a=b` 这两条语句依次执行。

可以给多变量赋值，如 `a,b=1,2` 意思是 `a=1,b=2` 依次执行。

#### 增量赋值

```python
+= -= *= /= //= %=
```

对字符串而言 `+=` 就是往后加字符，`*=` 就是把字符串复制多遍加起来，剩下两个符号对字符串无定义。

#### 圆括号

圆括号除了用在表达式里，可以用来调用函数，定义函数。

#### 优先级

从低到高依次为

```python
=
or
and
not
< > <= >= == !=
+ - 
* / // %
()
```

### 数据类型

`bool`：只有 `True` 和 `False`。

`int`：高精度整数。

`float`: 与 c++ 中的 `double` 一致。

`str`： 字符串，**immutable**。  // 不可改变  

### 语句

#### 变量定义/赋值语句

`var_1 = (var_2 = ...) = value`

语法规则参见赋值运算符。

#### 表达式语句

`var += value` 或其他增量赋值

==运算时如果两个运算数类型不一致，按照 c++ 规则执行自动类型转换。==

#### 条件语句

```python
if expression_1:
	# code block
elif expression_2:
	# code block
else expression_3:
	# code block
```

`elif` 相当于 `else if`，`else` 可以没有。

#### 循环语句

```python
while expression:
	# code block
```

#### 跳转语句

```python
break
return
continue
```

### 函数

#### 函数定义

```python
def func(parameters):
	# code block
```

参数列表如 `a,b,c`，变量名之间用逗号分隔，可以为空。

有些变量可以有默认值，但是都必须出现在无默认值的变量后面。

#### 函数调用

```python
func(parameters)
```

函数调用必须出现在该函数定义后。

参数有两种形式：keyword 和 positional。

Keyword argument 比如 `foo(a=1,b=2)` 表示传入参数 `a` 的值为 `1`，`b` 的值 `2`。

Positional argument 是指 `foo(1,2)` 这样按照出现顺序指代参数。

若一个参数列表中同时有两种参数形式，那么 positional argument 必须出现在 keyword argument 之前。如 `foo(1,b=2)`。

#### 内建函数

`print`：输出，可以有任意个参数，逐个输出，中间用空格分隔。输出后换行。输出 `float` 保留 $6$ 位小数。输出字符串不要输出前后面的引号。如 `print("123",1.0)` 请输出 `123 1.000000`。

`int`：将 `float` 或 `bool` 或 `str` 转成 `int`。

`float`: 将 `int` 或 `bool` 或 `str` 转成 `float`。

`str`： 将 `int` 或 `float` 或 `bool` 转成 `str`。

`bool`： 将 `int` 或 `float` 或 `str` 转成 `bool`。对于 `str`，如果是 `""` 则为 `False`，否则为 `True`。

转型类函数都只有一个参数。

#### 函数递归

数据保证递归层数不超过 $2000$ 层。

## 评分相关

本次作业要求用 OOP 实现，若不按照要求会在 code review 扣除一定分数。

作业按数据总的通过比例给分。助教会下发部分数据在作业仓库中，还有另一部分数据不会提供。

这意味着你需要自己手写测试数据给自己测试，如果你对自己造的数据是否满足要求有疑问，请及时向助教询问。

如果有实现要求之外的功能或者语法（如报错、容器、自定义类等），可酌情加分，最多不超过本作业的总分。

## 提示

建议使用 antlr4 框架完成。助教组已经提供了现成的语法文件。

如果你想挑战自己，可以自己写 parser 或者自己重写语法文件。

## 其他

如果有不懂的文法规则，参考 `python3.g4`，以上面的规则为准。

如果有其他问题可以及时联系助教。



## 待办清单

### 功能部分

- [x] 变量栈空间管理
- [x] 函数定义与调用

### 表达式解析部分

- [x] `file_input: (NEWLINE | stmt)* EOF;`
  
- [x] `funcdef: 'def' NAME parameters ':' suite;`

- [x] `parameters: '(' typedargslist? ')';`
  
- [x] `typedargslist: (tfpdef ('=' test)? (',' tfpdef ('=' test)?)*);`
  
- [x] `tfpdef: NAME ;`

- [x] `stmt: simple_stmt | compound_stmt;`
  
- [x] `simple_stmt: small_stmt  NEWLINE;`
  
- [x] `small_stmt: expr_stmt | flow_stmt;`
  
- [x] `expr_stmt: testlist ( (augassign testlist) | ('=' testlist)*);  // 连等 加等/减等/...`
  
- [x] `augassign: ('+=' | '-=' | '*=' | '/=' | '//=' | '%=' );`
  
- [x] `flow_stmt: break_stmt | continue_stmt | return_stmt;`
  
- [x] `break_stmt: 'break';`

- [x] `continue_stmt: 'continue';`
  
- [x] `return_stmt: 'return' (testlist)?;`
  
- [x] `compound_stmt: if_stmt | while_stmt | funcdef ;`
  
- [x] `if_stmt: 'if' test ':' suite ('elif' test ':' suite)* ('else' ':' suite)?;`
  
- [x] `while_stmt: 'while' test ':' suite;`

- [x] `suite: simple_stmt | NEWLINE INDENT stmt+ DEDENT;`
  
- [x] `test: or_test ;`
  
- [x] `or_test: and_test ('or' and_test)*;`
  
- [x] `and_test: not_test ('and' not_test)*;`
  
- [x] `not_test: 'not' not_test | comparison;`
  
- [x] `comparison: arith_expr (comp_op arith_expr)*;`
  
- [x] `comp_op: '<'|'>'|'=='|'>='|'<=' | '!=';`
  
- [x] `arith_expr: term (addorsub_op term)*;`
  
- [x] `addorsub_op: '+'|'-';`
  
- [x] `term: factor (muldivmod_op factor)*;`
  
- [x] `muldivmod_op: '*'|'/'|'//'|'%';`
  
- [x] `factor: ('+'|'-') factor | atom_expr;`
  
- [x] `atom_expr: atom trailer?;`

- [x] `trailer: '(' (arglist)? ')' ;`
  
- [x] `atom: (NAME | NUMBER | STRING+| 'None' | 'True' | 'False' | ('(' test ')'));`
  
- [x] `testlist: test (',' test)* (',')?;  // 算式  eg： a,b  a  a+b`
  
- [x] `arglist: argument (',' argument)*  (',')?;`
  
- [x] `argument: ( test | test '=' test );`

### 报错部分

- [ ] 未定义的变量
- [ ] 运算时的非法类型

