# 字符串基础

字符串生活中最常见的数据类型，任何显示出来可查看的文本内容都是字符串，比如“`123456`”、“`abcdefg!@#$`”。

在一个网页上快速地查找某个内容，在一个名单里快速确定某个同学是否存在以及在第几行，都是字符串处理的任务。

## 字符数组

字符类型`char`构成的数组存储的一系列字符可以构成一个字符串，也是`C`语言管理字符串的基本形式。

### 基本定义与读写

字符数组通过标准输入`scanf`的`%s`占位符来读取。

```cpp
char a[9];
scanf("%s", a);
char b[9] = "abc";
char c[] = "def";
printf("%s %s %s\n", a, b, c);
```

之前我们学习过数组的概念，这里数组的变量名 `a` 是这个数组第一个元素的内存地址，读取一个字符串，就是用`%s`把字符串以一个内存地址为起始位置连续写入。

如果对上述代码输入“`cats`”，则`a`数组的内存存储情况会像这样：

| 相对`a`的内存偏移 | 0 | 1 | 2 | 3 | 4  | 5  | 6  | 7  | 8  |
|----------|---|---|---|---|----|----|----|----|----|
| 存储内容     | c | a | t | s | \0 | 随机 | 随机 | 随机 | 随机 |

注意到`cats`之后内存地址保存的是“`\0`”，它是一个转义符号，由`scanf`自动添加，表示字符串的结尾，是字符串必须保留的一部分，由此可见，一个长度为 $4$ 的字符串，需要占用的内存量是 $5$ 个 `char`。`\0`之后的内存所保存的内容，程序不关心，有的系统在定义数组的时候会将数组内存都初始化为`0`，但有的不会，这些位置可以是任何值，且原则上不会被读取，直到程序往这些地方写入确定的内容，并把`\0`推后，这些地址才会有意义。

字符串数组也可以用字符串常量初始化。如果不指定数组大小并用字符串常量初始化，那么字符串数组会根据字符串常量的大小加上`\0`来开辟内存。


### ASCII码

在目前（2024年）的操作系统中，字符类型`char`实际上存储的是一个有 $8$ 个二进制位的整数，即 $-128 \sim 127$ 之间的数，我们通常关心的是它在 $0 \sim 127$ 的取值，包括了键盘上的大多符号以及一些不可见的符号。比如 $65$ 就是大写字母 “`A`”， $97$ 就是小写字母 “`a`”，刚才提到的标志字符串末尾的转义字符 “`\0`” 的ASCII码等于 $0$。

既然 `char` 实际上是整数，那么它自然也可以做一些基本的整数运算，而在输出时，取决于 `printf` 使用的占位符，过程中会根据输出的格式进行类型强制转换，比如：

```cpp
printf("97 as int: %d\n97 as char: %c\n'a' as int: %d\n'a' as char: %c\n", 97, 97, 'a', 'a');
```

得到结果如下

```text
97 as int: 97
97 as char: a
'a' as int: 97
'a' as char: a
```

作为整数，也同样可以进行加减乘除运算，由于字母表在ASCII码中是连续的，这给我们处理 $26$ 个英文字母带来了些许便利，比如想要按顺序输出 $26$ 个大写字母，可以这样：

```cpp
for(int i = 0; i < 26; i ++) {
    printf("%c\n", 'A' + i);
}
```

根据大写字母 “`A`”是 $65$， 小写字母 “`a`” 是 $97$ 这个信息，也可以方便地做大小写转换：

```cpp
char a = 'A';
printf("Lower case of %c is %c\n", a, a + 32);
```

ASCII码表如下

| ASCII值 | 控制字符 | ASCII值 | 控制字符    | ASCII值 | 控制字符 | ASCII值 | 控制字符 |
|--------|------|--------|---------|--------|------|--------|------|
| 0      | `NUT`  | 32     | `(空格)` | 64     | `@`    | 96     | `` ` `` |
| 1      | `SOH`  | 33     | `!`       | 65     | `A`    | 97     | `a`    |
| 2      | `STX`  | 34     | `"`       | 66     | `B`    | 98     | `b`    |
| 3      | `ETX`  | 35     | `#`       | 67     | `C`    | 99     | `c`    |
| 4      | `EOT`  | 36     | `$`       | 68     | `D`    | 100    | `d`    |
| 5      | `ENQ`  | 37     | `%`       | 69     | `E`    | 101    | `e`    |
| 6      | `ACK`  | 38     | `&`       | 70     | `F`    | 102    | `f`    |
| 7      | `BEL`  | 39     | `,`       | 71     | `G`    | 103    | `g`    |
| 8      | `BS`   | 40     | `(`       | 72     | `H`    | 104    | `h`    |
| 9      | `HT`   | 41     | `)`       | 73     | `I`    | 105    | `i`    |
| 10     | `LF`   | 42     | `*`       | 74     | `J`    | 106    | `j`    |
| 11     | `VT`   | 43     | `+`       | 75     | `K`    | 107    | `k`    |
| 12     | `FF`   | 44     | `,`       | 76     | `L`    | 108    | `l`    |
| 13     | `CR`   | 45     | `-`       | 77     | `M`    | 109    | `m`    |
| 14     | `SO`   | 46     | `.`       | 78     | `N`    | 110    | `n`    |
| 15     | `SI`   | 47     | `/`       | 79     | `O`    | 111    | `o`    |
| 16     | `DLE`  | 48     | `0`       | 80     | `P`    | 112    | `p`    |
| 17     | `DCI`  | 49     | `1`       | 81     | `Q`    | 113    | `q`    |
| 18     | `DC2`  | 50     | `2`       | 82     | `R`    | 114    | `r`    |
| 19     | `DC3`  | 51     | `3`       | 83     | `S`    | 115    | `s`    |
| 20     | `DC4`  | 52     | `4`       | 84     | `T`    | 116    | `t`    |
| 21     | `NAK`  | 53     | `5`       | 85     | `U`    | 117    | `u`    |
| 22     | `SYN`  | 54     | `6`       | 86     | `V`    | 118    | `v`    |
| 23     | `TB`   | 55     | `7`       | 87     | `W`    | 119    | `w`    |
| 24     | `CAN`  | 56     | `8`       | 88     | `X`    | 120    | `x`    |
| 25     | `EM`   | 57     | `9`       | 89     | `Y`    | 121    | `y`    |
| 26     | `SUB`  | 58     | `:`       | 90     | `Z`    | 122    | `z`    |
| 27     | `ESC`  | 59     | `;`       | 91     | `[`    | 123    | `{`    |
| 28     | `FS`   | 60     | `<`       | 92     | `\`    | 124    | `\|`    |
| 29     | `GS`   | 61     | `=`       | 93     | `]`    | 125    | `}`    |
| 30     | `RS`   | 62     | `>`       | 94     | `^`    | 126    | `~`    |
| 31     | `US`   | 63     | `?`       | 95     | `_`    | 127    | `DEL`  |



特殊字符解释

|     |   |  |
|----------|-----------|----------|
| NUL空     | VT 垂直制表   | SYN 空转同步 |
| STX 正文开始 | CR 回车     | CAN 作废   |
| ETX 正文结束 | SO 移位输出   | EM 纸尽    |
| EOY 传输结束 | SI 移位输入   | SUB 换置   |
| ENQ 询问字符 | DLE 空格    | ESC 换码   |
| ACK 承认   | DC1 设备控制1 | FS 文字分隔符 |
| BEL 报警   | DC2 设备控制2 | GS 组分隔符  |
| BS 退一格   | DC3 设备控制3 | RS 记录分隔符 |
| HT 横向列表  | DC4 设备控制4 | US 单元分隔符 |
| LF 换行    | NAK 否定    | DEL 删除   |

### 字符串常量与变量

```cpp
char a = 'x';
char b[10] = "xyz";
char c[10] = "%c %s\n";
printf(c, a, b);
printf("!!xyz\n" + 2);
```

输出内容为

```text
x xyz
xyz
```

通过这个示例代码发现，平时我们用 `scanf` 与 `printf` 时，第一个用于格式化的参数其实就是一个字符串常量，那么我们也可以用符合格式的变量去替代它，即这里的 `c`。

**特别注意**： **字符**常量用单引号“`'`”包裹，**字符串**常量用双引号“`"`”包裹。


字符串常量也可以做内存地址偏移的操作。我们知道一个数组 `char c[10];`，变量`c`本身实际上是这个数组第 $1$个元素的内存地址，那么`c+1`自然就是这个数组的第 $2$个元素的内存地址。对于字符串常量`"!!xyz\n"`，如果做运算`"!!xyz\n" + 2`，就能得到这个字符串常量的第 $3$ 个元素的内存地址，即 `x` 对应的地址，当`printf("!!xyz\n" + 2);`时，`printf`接收到的就是 `"xyz\n"` 了。



### 字符串的常用函数

`C`语言的`string.h`头文件提供了丰富的字符串处理函数，掌握一些常用函数会极大提高开发效率。

以下是一些C语言字符串函数的使用实例：

1. `strlen`：计算字符串的长度。返回值为字符串的长度，类型为`size_t`。

    ```c
    char s[] = "1234567890";
    int ret = strlen(s);
    printf("%d\n", ret); // 输出：10
    ```

2. `strcpy`：复制一个字符串到另一个字符串中。返回值为目标字符串的指针。

    ```c
    char s1[20] = "xxxxxxxxxxx";
    char s2[] = "abcdef";
    char* ret = strcpy(s1, s2);
    printf("%s\n", ret); // 输出："abcdef"
    ```

3. `strcat`：字符串拼接。返回值为目标字符串的指针。

    ```c
    char s1[100] = "Hello";
    char s2[100] = " World!";
    strcat(s1, s2);
    printf("%s\n", s1); // 输出："Hello World!"
    ```

4. `strcmp`：字符串比较。如果两个字符串相等，返回0；如果第一个字符串大于第二个字符串，返回大于0的值，这个值是第一个字符串中第一个不相等的字符的ASCII值减去第二个字符串中对应字符的ASCII值；反之返回小于0的值。

    ```c
    char s1[] = "abc";
    char s2[] = "ABC";
    int ret = strcmp(s1, s2);
    printf("%d\n", ret); // 输出：32
    ```

5. `strchr`：查找字符串中第一次出现第 $2$个参数（字符）的位置的指针。如果字符未在字符串中出现则返回`NULL`。

    ```c
    char s[] = "abcabcabc";
    char* ret = strchr(s, 'b');
    printf("%s\n", ret); // 输出："bcabcabc"
    ```

6. `strstr`：检索第 $2$个字符串在第 $1$个字符串中首次出现的位置的指针，如果子串未在字符串中出现则返回`NULL`。

    ```c
    char s1[] = "abcabcabc";
    char s2[] = "bca";
    char* ret = strstr(s1, s2);
    printf("%s\n", ret); // 输出："bcaabcabc"
    ```

7. `strtok`：将一个字符串分割为多个子字符串，返回值为下一个子字符串的指针，如果没有更多的子字符串则返回`NULL`。如果分隔符在被分割字符串中连续出现，连续的若干个分隔符会被视为 $1$ 个分隔符。

    ```c
    char s[] = "abc,def,,ghi";
    char* delim = ",";
    char* p = strtok(s, delim);
    while(p != NULL) {
        printf("%s\n", p); // 输出："abc"，然后是"def"，最后是"ghi"
        p = strtok(NULL, delim);
    }
    ```

8. `strncpy`：拷贝源字符串的前`num`个字符至目标字符串，第一个参数是目标字符串，第二个参数是源字符串。返回值为目标字符串的指针。

    ```c
    char s1[20] = "xxxxxxxxxxx";
    char s2[] = "abcdef";
    int num = 3;
    char* ret = strncpy(s1, s2, num);
    printf("%s\n", ret); // 输出："abcxxxxxxxx"
    ```

9. `strncat`：将第二个参数的前`n`个字符追加到字符串结尾。返回值为目标字符串的指针。

    ```c
    char s1[100] = "Hello";
    char s2[100] = " World!";
    strncat(s1, s2, 3);
    printf("%s\n", s1); // 输出："Hello Wo"
    ```

10. `strncmp`：指定长度比较。如果两个字符串的前`n`个字符相等，返回0，对前`n`个字符的比较规则与 `strcmp`类似。

    ```c
    char s1[] = "abc";
    char s2[] = "ABC";
    int ret = strncmp(s1, s2, 2);   // 比较前 2 个字符
    printf("%d\n", ret);            // 输出：32
    ```



## STL的string

`C++`的STL中有一个`string`容器可以做一些简单的字符串操作，初学者可以把它理解为一个封装了字符串数组的结构体。


### 定义和初始化

`string`需要`C++`的头文件`#include<string>`，注意与`C`语言头文件`#include<string.h>`区分。

```cpp
#include<string>
std::string str1;           // 创建一个空字符串
std::string str2("Hello!"); // 从一个C风格字符串创建
std::string str3(str2);     // 从另一个string对象创建
std::string str4(10, 'a');  // 创建一个包含10个字符'a'的字符串
```

### 访问和修改字符

使用 `operator[]` 或 `at()` 函数来访问或修改字符串中的字符。例如：

```cpp
std::string str = "Hello, World!";

char ch1 = str[0]; // 访问第一个字符
str[0] = 'x';      // 修改第一个字符

char ch2 = str.at(0);   // 访问第一个字符
str.at(0) = 'y';        // 修改第一个字符
```

### 字符串长度

`size()` 或 `length()` 函数可以返回字符串的长度。`empty()` 函数可以检查字符串是否为空。

```cpp
std::string str = "Hello, World!";
// 使用 size() 或 length() 函数获取字符串长度
printf("Length of str: %lu\n", str.size());     // 输出：13
printf("Length of str: %lu\n", str.length());   // 输出：13
// 使用 empty() 函数检查字符串是否为空
printf("Is str empty? %s\n", str.empty() ? "Yes" : "No"); // 输出：No
str = ""; // 将 str 设置为空字符串
// 再次检查字符串是否为空
printf("Is str empty? %s\n", str.empty() ? "Yes" : "No"); // 输出：Yes
```

### string输入与输出

`C++`通常配合`STL`的`cin`与`cout`进行输入与输出，这需要头文件 `#include<iostream>`

```cpp
#include<iostream>
#include<string>

int main() {
    std::string str;
    std::cin >> str;
    std::cout << "你输入的字符串是：" << str << std::endl;
    return 0;
}
```

`C++`虽然兼容`C`语言的语法，但`scanf`与`printf`针对底层字符串数组使用，与`string`配合时需要做一些处理。在输入时，先将字符串保存在字符数组中，再用字符数组的数据初始化一个`string`，在输出时，通过`string`的`c_str()`成员函数获取底层的字符串数组进行输出。


```cpp
#include <cstdio>
#include <string>

int main() {
    char buf[100];
    scanf("%s", buf);
    std::string str(buf);
    printf("你输入的字符串是：%s\n", str.c_str());
    return 0;
}
```

### 添加和删除字符

`push_back()` 函数可以在字符串末尾添加一个字符，`pop_back()` 函数可以删除字符串末尾的字符。`append()` 或 `operator+=` 可以用来连接字符串。

```cpp
std::string str = "Hello";
// 使用 push_back() 在字符串末尾添加一个字符
str.push_back('!');
printf("After push_back: %s\n", str.c_str());   // 输出：Hello!
// 使用 pop_back() 删除字符串末尾的字符
str.pop_back();
printf("After pop_back: %s\n", str.c_str());    // 输出：Hello
// 使用 append() 连接字符串
str.append(", World!");
printf("After append: %s\n", str.c_str());      // 输出：Hello, World!
// 使用 operator+= 连接字符串
str += " How are you?";
printf("After operator+=: %s\n", str.c_str());  // 输出：Hello, World! How are you?
```

### 子字符串和字符查找

`substr()` 函数可以返回一个子字符串。`find()` 和 `rfind()` 函数可以用来查找字符或子字符串。

```cpp
std::string str = "Hello, World!";

// 使用 substr() 返回一个子字符串
std::string sub = str.substr(0, 5);
printf("Substr: %s\n", sub.c_str());    // 输出：Hello

// 使用 find() 查找字符或子字符串
size_t pos = str.find("World");
if (pos != std::string::npos) {
    printf("Find: %lu\n", pos);         // 输出：7
} else {
    printf("Find: Not found\n");
}

// 使用 rfind() 查找字符或子字符串
pos = str.rfind('o');
if (pos != std::string::npos) {
    printf("Rfind: %lu\n", pos);        // 输出：8
} else {
    printf("Rfind: Not found\n");
}
```
### 字符串比较

字符串的大小关系默认按ASCII码的编号来比，从两字符串的开头依次比较，第一次遇到不相同字符时，ASCII码小的那个字符串更小。对于`string`，可以使用 `operator==`，`operator!=`，`operator<`，`operator>`，`operator<=` 和 `operator>=` 来比较两个字符串。

```cpp
std::string str1 = "Hello";
std::string str2 = "World";
std::string str3 = "Hello";
if (str1 == str3) {
    printf("str1 == str3\n");
}
if (str1 != str2) {
    printf("str1 != str2\n");
}
if (str1 < str2) {
    printf("str1 < str2\n");
}
if (str2 > str1) {
    printf("str2 > str1\n");
}
```

### 一些其他操作


1. 插入字符`insert()`

    ```cpp
    std::string str = "Hello, World!";
    str.insert(0, "Greetings from ");   // 在位置0插入子字符串
    printf("%s\n", str.c_str());        // 输出：Greetings from Hello, World!
    ```

1. 删除字符`erase()`
    ```cpp
    std::string str = "Hello, World!";
    str.erase(0, 5);                // 删除位置0开始的5个字符
    printf("%s\n", str.c_str());    // 输出：, World!
    ```

1. 替换字符`replace()`

    ```cpp
    std::string str = "Hello, World!";
    str.replace(0, 5, "Hi");        // 替换位置0开始的5个字符
    printf("%s\n", str.c_str());    // 输出：Hi, World!
    ```

在实际编程中，还会用到其他函数，可以查看[官方文档](https://zh.cppreference.com/w/cpp/string/basic_string)了解更多。

## 字符串整行读入

在C/C++中，使用`scanf("%s", str)`只能读取不带空白字符（如空格“` `”、制表符“`\t`”）的字符串，但有时候需要一次性读入带空白字符的一整行字符，像这样：

```text
Hello, today is sunny!
Yes, its great!
```
这时最简单粗暴的方法是`while((ch = getchar()) != '\n')`，逐字符写入数组，但一方面这样做略嫌麻烦，另一方面由于`Windows`、`Linux`、`Mac`等不同系统在换行位置的占位符不尽相同，也会让整行读入的末尾兼容性有限，出现问题。

1. 已淘汰的旧标准函数`gets()`

    依赖头文件 `#include<stdio.h>`，`gets()`可以从标准输入中连续读取字符直到出现换行符或文件末尾，读取结束后，末尾的 `\n` 会被替换为 `\0` 。由于该函数不检查数组边界，被认为不安全，所以在`C11`标准中被删除，但在一些比较古老的OJ中仍然只支持旧的编译器，`gets()`仍可使用。

    ```cpp
    char buf[100];
    while(gets(buf)) {   // 持续按行读取直到文件末尾
        // 进行后续操作
    }
    ```

1. `fgets()`

    `fgets()`可以视为`gets()`的优化版。它多了两个参数，一个是输入流，可以指定为标准输入`stdin`，也可以指定为文件流，另一个指定最大写入字符数。

    需要注意与`gets()`不同的是，`fgets()`会将读取到的`\n`保留在数组中，并在其后添加`\0`，这样读入的字符串会比我们平时预期的多一个字符，**需要手动处理**。

    ```C++
    char buf[100];
    while(fgets(buf, sizeof(buf), stdin)) {   // 持续按行读取直到文件末尾
        buf[strlen(buf) - 1] = 0;       // 手动去掉末尾的\n
        // 进行后续操作
    }
    ```
1. `scanf()`也有办法

    其实 `scanf()` 也可以整行读入，`scanf()`有两个比较少用的转换说明符`%[]`和`*`

    ```C++
    scanf("%[^\n]%*c", buf);
    ```

    - `%[]`的功能与`%s`类似，但其只匹配`[]`中包含的字符，如`%[0-9]`为仅匹配数字，`%[a-zA-Z]`为仅匹配大小写字母。如果`[`后紧跟`^`则意为取补集，如`%[^0-9]`为匹配除数字以外的所有字符。
    - `*`为抑制字符，跟在`%`后可以使当前转换说明符只匹配但不赋值给变量。

    在上面的代码中，`%[^\n]`用于将`\n`之前的所有字符写入str中，`%*c`用于读取剩下来的`\n`但不赋给变量。

    ```C++
    char buf[100];
    while(scanf("%[^\n]%*c", buf) != EOF) { // 持续按行读取直到文件末尾
        // 进行后续操作
    }
    ```

1. `STL`的`string`可以用`std::getline()`

    在引入头文件`iostream`和`string`后，就可以使用一种省心又省力的行读取办法：

    ```cpp
    std::string st;
    while (std::getline(std::cin, st)) {    // 持续按行读取直到文件末尾
        // 进行后续操作
    }
    ```
    

### 注意事项

#### 对于`C`语言的处理方式

在整行读入前，需要先考虑上一行的`\n`是否残留并将其单独处理掉。

反面教材：

```cpp
#include<stdio>
int a, b;
char s[100];
int main() {
    scanf("%d%d", &a, &b);
    scanf("%[^\n]%*c", s); 
    printf("%s", s);
    return 0;
}
```

输入：

```
2 3
Hello world!
```

输出：

```
（无输出）
```

第一条`scanf()`语句读取两个整数后，留下了行末的`\n`未处理，被第二条`scanf()`的读取方式并不会跳过`\n`，导致`s`中遇到前一行的`\n`就停止了。

在极少数情况下，OJ中的数据文件以`\r\n`作为每行的结尾，而上述各种方法都只会将`\r`视作普通字符，`fgets()`读取的字符串会以`\r\n`结尾，`scanf()`、`gets()`则会在结尾处会多出`\r`，都没有完美地处理这种情况。

不过，在明确OJ中以`\r\n`作为每行的结尾时，可以对`scanf()`稍加修改。

```cpp
scanf("%[^\r]%*c%*c", buf);  // 此时读入以\n结尾的内容会出错
```

否则，在不确定OJ是否以`\r\n`结尾时，建议按`\n`的情况处理。如果追求兼容性，可以使用`getchar()`逐字符判断。

#### 对于利用`string`的处理方式

需要注意的是，一旦决定使用`std::cin`和`std::cout`来作为输入输出方式，那就需要考虑解除流同步和流绑定，因为`std::cin`和`std::cout`默认会与`scanf()`和`printf()`的缓冲区进行同步，以保证可以在代码中同时使用这四种输入输出方法而不出现乱序输入输出的情况，但这会大大拖慢`std::cin`和`std::cout`的速度，我们需要在程序开头加入这一段代码来解除同步和绑定：

```cpp
std::ios::sync_with_stdio(false);
std::cin.tie(0);
std::cout.tie(0);
```

在解除同步和绑定后，`std::cin`和`std::cout`就不能与`scanf()`和`printf()`同时使用了，但是换来了输入输出速度。

### 一个封装实践

```cpp
#include<cstdio>
#include<cstring>
int a, b;
char buf[999];

char *gets_trim(char buffer[], int bufsize) {
    if(fgets(buffer, bufsize, stdin)) {
        int len = strlen(buffer);
        while(len > 0 && buffer[len - 1] == '\n' || buffer[len - 1] == '\r') {
            buffer[-- len] = 0;
        }
        return buffer;
    }
    return NULL;
}

int main() {
    scanf("%d %d", &a, &b);
    gets_trim(buf, sizeof(buf));    // 消除前面的\n
    while(gets_trim(buf, sizeof(buf))) {
        printf("%s\n", buf);
    }
    return 0;
}
```