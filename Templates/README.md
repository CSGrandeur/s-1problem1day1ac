## 贡献模板要求

### 仓库现有模板说明

现有模板仅表示目录结构以及后续更新的目录格式。代码实现不一定符合规范。

代码规范以下文为准。

### 实现要求

封装为 `struct`。

对于潜在的数据规模动态的问题尽可能使用容器动态使用内存，如 `vector<int>`。

不使用流，所有输入输出使用 `stdio.h`。

任何说明内容务必简短，可使用基础 markdown 语法，不可出现扩展markdown、latex公式等语法。

### 常用约定

- 默认带`stdio.h`、`string.h`、`stdlib.h`、`algorithm`等头文件，需要其他头文件时需在模板中给出
- `maxn` 为题目主规模，`maxm` 为辅规模，模板中不出现`const maxn=` 等定义，可在模板前对数据设定进行说明。

其余常用变量约定可发 pull request 更新本文档。

### 代码规范

除上文约定外，其余参考google C++ 规范：https://google.github.io/styleguide/cppguide.html

中文版：https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/contents/
