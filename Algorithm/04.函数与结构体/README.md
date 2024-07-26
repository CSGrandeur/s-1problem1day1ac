# 函数与结构体

## 函数基本概念

### 什么是函数

- 一段具有特定功能的代码块
- 可以执行某项特定任务并返回结果

例如输入`a`与`b`求`a+b`这件事，不使用函数：

```cpp
#include<cstdio>
int main() {
    scanf("%d%d", &a, &b);
    printf("%d\n", a + b);
    return 0;
}
```

使用函数：

```cpp
#include<cstdio>
int Add(int a, int b) {
    return a + b;
}
int main() {
    scanf("%d%d", &a, &b);
    printf("%d\n", Add(a, b));
    return 0;
}
```

### 函数的定义

函数的定义：

1. 返回值类型：定义返回类型，`void`则表示不返回内容；
1. 函数名：用该名字调用函数；
1. 参数类型： $0$个或多个参数，参数之间用“`,`”隔开；
1. 参数名：每个参数类型之后跟参数名，在函数里可以使用这些参数；
1. 函数内逻辑：执行特定任务；
1. 返回值：如果返回值类型不是`void`，则必须用`return`返回一个变量的值。

```cpp
<返回值> <函数名>(<参数类型1> <参数名1>, <参数类型2> <参数名2>, ...) {
    <函数逻辑>
    return <返回值>
}
```

如果`<返回值>`不是`void`，则函数结尾必须`return <返回值>`

无返回值的函数示例：

```cpp
void Hello() {
    printf("Hello, World!");
}
int main() {
    Hello();
    return 0;
}
```

注意事项：

- 推荐将自定义函数写在`int main`的上面，否则编译器在执行`main()`的时候不知道这个函数；
- 函数的名称和变量名都是大小写敏感的，定义`Add()`就不能用`add()`去调用；


### 函数的调用与参数传递

函数通过函数名调用，格式是 `<函数名> (<参数1>, <参数2>, ...);`

1. 没有返回值的函数，直接调用
    ```cpp
    int main() {
        Hello();
        return 0;
    }
    ```
1. 有返回值的函数，将返回值赋值给变量，或直接使用返回值
    ```cpp
    int main() {
        // ...
        int c = Add(a, b);
        printf("%d\n", Add(a, b));
        return 0;
    }
    ```
1. 参数要与变量类型对应，定义`int`则对应传入`int`，定义`double`则对应传入`double`
    ```cpp
    int Add(double a, double b) {
        return a + b;
    }
    int main() {
        double x, y;****
        // scanf ...
        printf("%f\n", Add(x, y));
        // 错误示例：char x, y; Add(x, y); Add的参数是double，不能传char参数
    }
    ```

**函数内对参数的修改，不会影响到调用函数所传的变量**

尝试

```cpp
void Change(int a) {
    printf("Test1: %d\n", a);
    a += 2;
    printf("Test2: %d\n", a);
    a *= 3;
    printf("Test3: %d\n", a);
}
int main() {
    int x = 10;
    Change(x);
    printf("Test4: %d\n", x);
    return 0;
}
```

输出为 

```text
Test1: 10
Test2: 12
Test3: 36
Test4: 10
```

可以这样认为，将`main`中的`x`传入`Change`函数时，函数得到的是`x`的一个副本`a`，对`a`的任何修改都是对副本的修改，不会影响`x`本来的值。

这里传给函数的`x`称为“实际参数”，函数得到的`a`称为“形式参数”。

在`C++`中，如果想让函数得到变量“本体”，可使用叫做“引用”的语法，即在参数前加“`&`”符号：

```cpp
void Change(int &a) {
    printf("Test1: %d\n", a);
    a += 2;
    printf("Test2: %d\n", a);
    a *= 3;
    printf("Test3: %d\n", a);
}
int main() {
    int x = 10;
    Change(x);
    printf("Test4: %d\n", x);
    return 0;
}
```

输出为 

```text
Test1: 10
Test2: 12
Test3: 36
Test4: 36
```

两份代码唯一区别是 `Change`函数定义时，其参数定义由`int a`改为了`int &a`，表示对传入参数的引用，这样对`a`的修改会对本体`x`生效。


**函数的返回可以“截断”后续的逻辑**

```cpp
int f(int a, int b) {
    if(a < b) return a;
    return b;
}
int main() {
    printf("%d\n", f(3, 5));
    printf("%d\n", f(8, 5));
    return 0;
}
```

当满足 `a<b`时，`return a`的逻辑会被执行，函数中任何`return`被执行时，之后的代码都不会再被执行。

当不满足`a<b`时，代码会继续向下执行到`return b`。

### 函数的意义

- 复用同一段代码处理相同的任务
- 让程序更加模块化，更易于理解和维护

例：分别求 $1\sim 10$、 $200\sim 300$的和并输出

不使用函数：

```cpp
int main() {
    int sum;

    sum = 0;
    for(int i = 1; i <= 10; i ++) {
        sum += i;
    }
    printf("1~10的和=%d", sum);

    sum = 0;
    for(int i = 200; i <= 300; i ++) {
        sum += i;
    }
    printf("200~300的和=%d", sum);

    return 0;
}

使用函数：

```cpp
int CalSum(int start, int end) {
    int sum = 0;
    for(int i = start; i <= end; i ++) {
        sum += i;
    }
    printf("%d~%d的和=%d", start, end, sum);
    return sum;
}
int main() {
    CalSum(1, 10);
    CalSum(200, 300);
    return 0;
}
```

使用函数精简了代码，且逻辑清晰。

如果这时任务改成，“求一段数从第一个数开始，奇数位置上的和”，如果没有使用函数，则每个执行此任务的地方都要修改代码。当使用函数时，只需要修改`CalSum`这个函数内的`i ++`改成`i += 2`就可以，代码维护变得简单。

## 函数的递归

### 递归的定义

```cpp
名词定义 递归() {
    return 递归();
}
```

递归即函数自己调用自己，或若干个函数在互相调用。当然上面的示例看起来永远不会结束，它是一个无限递归。

通常用递归来解决可以被分解的问题，在问题足够小的时候有一个终点。比如给出 $n$，求 $n$的阶乘，即 $n!=1\times 2\times, \dots, n$，递归可以这样写：

```cpp
int f(int x) {
    if(x == 0) return 1;
    return f(x - 1) * x;
}
```

所以一个合理的递归，应该有两部分组成

1. 对问题的分解：调用函数本身去解决一个“更小”的问题
    - 这里`f(x - 1)` 就是对问题的分解，递归地去处理规模更小的`x-1`的阶乘，并乘以`x`得到`f(x)`的解
2. 递归的终点：当问题足够小时，以一个确定的逻辑返回，而不再继续分解
    - `if(x == 0) return 1`就是在做这件事，确保递归不会无限进行下去，`0`是一个足够小的问题

### 递归的意义

实际上，任何递归逻辑都可以用非递归的代码去完成，比如阶乘，用一个循环将 $1\sim n$累乘起来也能实现。

但在一些复杂问题中，递归能帮助我们更好地理清思路，将问题抽丝剥茧，要比从全局思考正确步骤更为容易。

以汉诺塔问题为例，有三根柱子，一根柱子上有 $n$个大小不同的盘子，将所有的盘子移动到另一根柱子上，在移动过程中，任何时候都不能让大盘子在小盘子上面。

![三个盘子的汉诺塔](resource/hanoi.gif)

如果把柱子从左到右命名为 $A$、 $B$、 $C$，以“`A to C`”这样的指令表示把 $A$最顶部的盘子挪到 $C$的最顶部，当一开始 $A$柱有 $n$个盘子，以一系列指令给出将所有盘子在约束条件下从 $A$移到 $C$的方案。

不用递归的话，面对这个问题还是很懵的，那么定义一个这样的函数：

```cpp
void Hanoi(int n, char x, char y, char z) {
    // TODO
}
```

表示把`n`个盘子从变量`x`所表示的柱子，利用`y`作为辅助柱子，以符合题目要求的方式全部移动到`z`所表示的柱子上。

那么第一次调用这个函数应该是这样：

```cpp
Hanoi(n, 'A', 'B', 'C');
```

可以这样分解该问题：

1. $A$上的 $n-1$个盘子通过 $C$先移到 $B$
2. $A$上最底部的那个盘子直接移到 $C$，即输出“`A to C`”
3. $B$上当前的 $n-1$个盘子通过 $A$移到 $C$， $C$上在第2步放的那个最大的盘子完全不影响这 $n-1$个盘子的任何移动

这3步就完成了整个任务。而第1步和第3步是相同的问题，只是问题规模减小了1，且起点柱子和终点柱子不同了而已，他们可以递归地调用`Hanoi`

完整的递归代码这样实现：

```cpp
void Hanoi(int n, int x, int y, int z) {
    if(n == 1) {
        printf("%c to %c\n", x, z); // x 和 z 具体是哪个柱子这里并不重要
    } else {
        Hanoi(n - 1, x, z, y);      // 执行第 1 步
        printf("%c to %c\n", x, z); // 执行第 2 步
        Hanoi(n - 1, y, x, z);      // 执行第 3 步
    }
}
int main() {
    int n = 3;
    Hanoi(n, 'A', 'B', 'C');
    return 0;
}
```

### 递归栈

当一个函数被调用或被递归地调用，调用结束时代码还要继续往下进行，操作系统怎么“恢复现场”呢？

比如

```cpp
int f(int x) {
    return x == 0 ? 1 : f(x - 1) * x;
}
int main() {
    int n = 10, a = 5;
    printf("%d\n", f(n));
    // 当刚刚的 f(n) 运行结束，操作系统如何回到这里，并知道 n==10，a==5？
    n ++;
    a += n;
    printf("%d %d\n", n, a);
}
```

这就有一个“栈内存”的概念，每当调用函数，在进入函数执行前，系统会“保护现场”，把所有信息保存到一块内存中。当函数执行完毕，会从这块栈内存把所有信息读回来“恢复现场”并继续执行接下来的代码。

通常系统的栈内存有一个固定大小，当递归时，每次调用都会“保护现场”，占用一些栈内存，那么递归得太深，就可能把栈内存用尽，程序就会出错，这种情况称为“栈溢出”。

比如递归求一个很大的阶乘

```cpp
#include<cstdio>
int f(int n) {
    int ret;
    ret = n == 0 ? 1 : f(n - 1) * n;
    printf("now: %d\n", n);
    return ret;
}
int main() {
    int n;
    n = 1000000000;
    printf("%d\n", f(n));
    return 0;
}
```

因为现代编译器会做一些优化来尽可能避免栈溢出，这里作为栈溢出的例子增加了一些无用代码来确保溢出不会被优化掉，这方面的知识大家以后会了解。

栈溢出的知识点告诉我们，在思考一个问题解法的时候，如果需要的递归深度离谱的高，就要慎重考虑一下是不是最好的方案了。

当然还有一个更常见的情况，也是建议，较大的数组放在`main`函数外面

```cpp
// 推荐做法：
int a[10000];
int main() {
    // ...
}
// 不建议做法：
int main() {
    int a[10000];
    // ...
}
```

函数内的一切变量都开在“栈内存”中，过大会导致栈溢出。而函数外的变量会开在“堆内存”中，这就是其他话题了，此处不再赘述。

## 结构体

### 结构体的概念

将多种变量封装到一起从而自定义一种类型。

一个“学生”有学号、年龄、身高、体重等信息，如果一个程序想以“学生”作为对象来做数据处理，或者以某种条件筛选学生并返回学生信息：

不使用结构体

```cpp
// 输出学生信息
void PrintStu(int ID, int age, int height, int weight) {
    printf("%d %d %d %d\n", ID, age, height, weight);
}
// 获取特定ID的学生信息并输出
int ID_list[1000], age_list[1000], height_list[1000], weight_list[1000];
void Search(int ID_search) {
    int id_i = 0;
    for(; id_i < 1000; id_i ++) {
        if(ID_list[id_i] == ID_search) {
            break;
        }
    }
    PrintStu(ID_list[id_i], age_list[id_i], height_list[id_i], weight_list[id_i]);
}
```

这还只是简单的需求。如果一个学生要记录更多维度的信息如姓名、住址、手机号等等等等，代码的编写将十分繁琐且不便维护。

将一个学生作为一个统一的对象，把信息打包封装：

```cpp
struct Student{
    int ID, age, height, weight;
};
Student s[1000];
void Search(int ID_search) {
    int id_i = 0;
    for(; id_i < 1000; id_i ++) {
        if(s[id_i].ID == ID_search) {
            break;
        }
    }
    PrintStu(s[id_i].ID, s[id_i].age, s[id_i].height, s[id_i].weight);
}
```

这样`Student` 就可以作为一个自定义的数据类型，去定义变量或数组，每个该类型的变量都会包含“学生”这一概念的各种信息。

用结构体定义的变量称为该结构体的“对象”，用“`.`”来调用对象内的变量。

### 成员函数

如果一些功能只与结构体自身有关，可以理解为这个结构体自身的某种功能，那么可以将函数定义为结构体的“成员函数”，同样用“`.`”来调用对象内的成员函数，成员函数可以直接使用和修改对象内的变量。

```cpp
struct Student{
    int ID, age, height, weight;
    void Init() {
        ID = age = height = weight = 0;
    }
    void Print() {
        printf("%d %d %d %d\n", ID, age, height, weight);
    }
};
Student s[1000];
void Search(int ID_search) {
    // ...
    s[id_i].Print();
}
```

### 运算符的重载

`int`、`double`这些类型天然具备的 `+,-,*,/,>,<` 等操作，作为自定义变量类型的结构体并没有良好的定义。

我们当然可以用函数实现结构体之间的各种运算，但如果能用这些运算符，会让代码逻辑更为清晰。

比如两个学生的大小用年龄的大小关系来定义，函数可以这样实现：

```cpp
struct Student{
    int ID, age, height, weight;
    bool Bigger(const Student &that) {
        // 当前对象比 that 对象的 age 更大则返回 true
        return age > that.age;
    }
};
Student a, b;
int main() {
    a.age = 10;
    b.age = 9;
    if(a.Bigger(b)) {
        printf("a is bigger\n");
    } else {
        printf("a is not bigger\n");
    }
    return 0;
}
```

而重载运算符，就可以在结构体的对象之间执行相应的运算，比如

```cpp
struct Student{
    int ID, age, height, weight;
    bool operator>(const Student &that) {
        // 当前对象比 that 对象的 age 更大则返回 true
        return age > that.age;
    }
};
Student a, b;
int main() {
    a.age = 10;
    b.age = 9;
    if(a > b) {
        printf("a is bigger\n");
    } else {
        printf("a is not bigger\n");
    }
    return 0;
}
```


