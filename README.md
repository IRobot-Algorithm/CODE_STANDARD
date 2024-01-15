# 规范

## 1. 提交规范

### 1.1 git Commit message  格式

每次提交，Commit message 都包括三个部分：Header，Body 和 Footer。

```markdown
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```

 **其中，Header 是必需的，Body 和 Footer 可以省略。**

不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。

#### 1.1.1 Header

 Header部分只有一行，包括三个字段：`type`（必需）、`scope`（可选）和`subject`（必需）。

##### (1) type 

```markdown
- feat：新功能（feature）
- fix：修补bug
- docs：文档（documentation）
- style： 格式（不影响代码运行的变动）
- refactor：重构（即不是新增功能，也不是修改bug的代码变动）
- test：增加测试
- chore：构建过程或辅助工具的变动
```

如果`type`为`feat`和`fix`，则该 commit 将肯定出现在 Change log 之中。其他情况（`docs`、`chore`、`style`、`refactor`、`test`）由你决定，要不要放入 Change log，建议是不要。

#####  (2) **scope** (optional)

 `scope`用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

##### (3) **subject (optional)**

 `subject`是 commit 目的的简短描述，不超过50个字符。

```markdown
- 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
- 第一个字母小写
- 结尾不加句号（.）
```

#### 1.1.2 Body

Body 部分是对本次 commit 的详细描述，可以分成多行。下面是一个范例。

```markdown
More detailed explanatory text, if necessary.  Wrap it to  about 72 characters or so. 
Further paragraphs come after blank lines.
- Bullet points are okay, too
- Use a hanging indent
```

有两个注意点。

（1）使用第一人称现在时，比如使用`change`而不是`changed`或`changes`。

（2）应该说明代码变动的动机，以及与以前行为的对比。

#### 1.1.3 Footer (optional)

Footer 部分只用于两种情况。

**（1）不兼容变动**

如果当前代码与上一个版本不兼容，则 Footer 部分以`BREAKING CHANGE`开头，后面是对变动的描述、以及变动理由和迁移方法。

> ```bash
> BREAKING CHANGE: isolate scope bindings definition has changed.
> 
>  To migrate the code follow the example below:
> 
>  Before:
> 
>  scope: {
>    myAttr: 'attribute',
>  }
> 
>  After:
> 
>  scope: {
>    myAttr: '@',
>  }
> 
>  The removed `inject` wasn't generaly useful for directives so there should be no code using it.
> ```

**（2）关闭 Issue**

如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。

> ```bash
> Closes #234
> ```

也可以一次关闭多个 issue 。

> ```bash
> Closes #123, #245, #992
> ```

#### 1.1.4  Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以`revert:`开头，后面跟着被撤销 Commit 的 Header。

> ```bash
> revert: feat(pencil): add 'graphiteWidth' option
> 
> This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
> ```

Body部分的格式是固定的，必须写成`This reverts commit <hash>.`，其中的`hash`是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的`Reverts`小标题下面。

#### 1.1.5 提交示例

```markdown
- docs : add email.

- docs(Detector) : add mising dependencies. 
# 此处scope 为Detector模块

- feat($compile) : add tracking model in the detector moudle.
# 此处scope 为影响编译

- fix: use 'false' as default for 'use_sim'. Closes #234.
```

### 1.2 提交其他注意

- 不要提交编译后的**二进制文件**等
- 不要提交.vscode等项目配置文件
- 不要一次**大量**提交
- ...

## 2. 代码规范

所有的规定虽然麻烦，但都是为了防止一座屎山的诞生。

###  2.1 项目文件树基础结构

对于ROS2项目，遵循ROS2的风格即可。对于非ROS2项目，使用下面的文件树风格

```
├── docs  
│   └── READEME文档使用的图片或其他资源    
├── build  
├── CMakeLists.txt  
├── configure	  
│   └── settings.xml	# 参数文件  
├── include				# 头文件目录  
│   ├──  package1		# 模块名  
│   │     └── demo1.hpp  
│   └── package2  
│         └── demo2.hpp  
├── README.md		# 说明文档  
└── src				# 源文件目录  
	├── package1       
	│   └── demo1.cpp    
	├── package2   
	│   └── demo2.cpp    
	└── main.cpp	# 主程序  
```



###  2.2 命名约定

####  2.2.1 通用命名规则

**总述**

函数命名, 变量命名, 文件命名要有描述性; 少用缩写

**说明**

尽可能使用描述性的命名, 别心疼空间, 毕竟相比之下让代码易于新读者理解更重要。 不要用只有项目开发者能理解的缩写, 也不要通过砍掉几个字母来缩写单词。

```cpp
int price_count_reader;    // 无缩写
int num_errors;            // "num" 是一个常见的写法
int num_dns_connections;   // 人人都知道 "DNS" 是什么
```

```c++
int n;                     // 毫无意义.
int nerr;                  // 含糊不清的缩写.
int n_comp_conns;          // 含糊不清的缩写.
int wgc_connections;       // 只有贵团队知道是什么意思.
int pc_reader;             // "pc" 有太多可能的解释了.
int cstmr_id;              // 删减了若干字母.
```

注意, 一些特定的广为人知的缩写是允许的, 例如用 `i` 表示迭代变量和用 `T` 表示模板参数.

####  2.2.2 文件命名

**总述**

文件名要全部小写, 可以包含下划线 (`_`) ，对于一些特殊规定的文件除外，如：`CMakeLists.txt` ,`README.md`

**说明**

可接受的文件命名示例:

- `my_useful_class.cpp`
- `myusefulclass.cpp`

C++ 文件要以 `.cpp` 结尾, 头文件以 `.hpp` 结尾.

####  2.2.3 类型命名

**总述**

类型名称的每个单词首字母均大写, 不包含下划线: `MyExcitingClass`, `MyExcitingEnum`.

**说明**

所有类型命名 —— 类, 结构体, 类型定义 (`typedef`), 枚举, 类型模板参数 —— 均使用相同约定, 即以大写字母开始, 每个单词首字母均大写, 不包含下划线. 例如:

```c++
// 类和结构体
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// 类型定义
typedef hash_map<UrlTableProperties *, string> PropertiesMap;

// using 别名
using PropertiesMap = hash_map<UrlTableProperties *, string>;

// 枚举
enum UrlTableErrors { ...
```

####  2.2.4. 变量命名

**总述**

变量 (包括函数参数) 和数据成员名一律小写, 单词之间用下划线连接. 类的成员变量以下划线结尾, 但结构体的就不用, 如: `a_local_variable`, `a_struct_data_member`, `a_class_data_member_`.

**说明**

#####   普通变量命名

举例:

```c++
string table_name;  // 好 - 用下划线.
string tablename;   // 好 - 全小写.

string tableName;  // 差 - 混合大小写
```

##### 类数据成员

不管是静态的还是非静态的, 类数据成员都可以和普通变量一样, 但要接下划线.这样是为了更好地区分你的类成员函数中的临时变量与成员变量

```c++
class TableInfo {
  ...
 private:
  string table_name_;  // 好 - 后加下划线.
  string tablename_;   // 好.
  static Pool<TableInfo>* pool_;  // 好.
};
```

##### 结构体变量

不管是静态的还是非静态的, 结构体数据成员都可以和普通变量一样, 不用像类那样接下划线:

```c++
struct UrlTableProperties {
  string name;
  int num_entries;
  static Pool<UrlTableProperties>* pool;
};
```

####  2.2.5 函数命名

**总述**

常规函数使用大小写混合或全小写+下划线: `myExcitingFunction()`或`set_my_exciting_member_variable()`

**说明**

大小写混合方式中，要求首单词字母小写，后续单词首字母大写

####  2.2.6 枚举命名

**总述**

枚举的命名应当和宏一致，全大写+下划线:  `ENUM_NAME`.

```c++
enum AlternateUrlTableErrors {
    OK = 0,
    OUT_OF_MEMORY = 1,
    MALFORMED_INPUT = 2,
};
```

###  2.3. 头文件

1. 头文件里一般不添加函数的具体实现，除非函数代码段的内容能在10行以内，一般在声明处实现的函数都称为内联(`inline`)函数。

2. 所有的头文件命名均以`.hpp`结尾

3. 头文件均存放在`include/package/`路径下，`package`为自己定义的功能包或模块名

####  2.3.1 #define保护

所有头文件都应该有 `#define` 保护来防止头文件被多重包含, 命名格式当是:`<PACKAGENAME>__<FILE>_HPP_` ，如`rmos_detector`功能包中的`detector_node.hpp`

```c++
#ifndef RMOS_DETECTOR__DETECTOR_NODE_HPP_
#define RMOS_DETECTOR__DETECTOR_NODE_HPP_
···
#endif // RMOS_DETECTOR__DETECTOR_NODE_HPP_
```

####  2.3.2 `#inlcude`的路径及顺序

项目内头文件应按照项目源代码目录树结构排列, 避免使用 UNIX 特殊的快捷目录 `.` (当前目录) 或 `..` (上级目录). 例如, `RMOS/src/rmos_detector/include/rmos_detector/detector_node.hpp` 应该按如下方式包含:

```c++
#include "rmos_detctor/detector_node.hpp"	// "功能包/中间目录/头文件"
```

对于头文件的`#include`应当遵循下列顺序：

> 1.  系统文件
> 2.  其他库的头文件
> 3.  本项目内头文件

除此之外，对于用于实现其他头文件的`.cpp`文件，如`src/Detector.cpp`实现了`include/detector/Detector.hpp`中的功能，那么在该`.cpp`中应当把`#include "detector/Detector.hpp"`最高优先顺序

####  2.3.3 关于<>和""的使用

除本项目内的头文件使用""外，其他类型头文件均使用<>

```c++
#include <iostream>
#include <string>
#include <opencv2/opencv.hpp>

#include "rmos_detector/detector.hpp"
```

###  2.4. 注释

注释虽然写起来很痛苦, 但对保证代码可读性至关重要. 下面的规则描述了如何注释以及在哪儿注释. 当然也要记住: 注释固然很重要, 但最好的代码应当本身就是文档. 有意义的类型名和变量名, 要远胜过要用注释解释的含糊不清的名字.

你写的注释是给代码读者看的, 也就是下一个需要理解你的代码的人. 所以慷慨些吧, 下一个读者可能就是你

####  2.4.1 函数注释

**说明**

#####  函数声明

基本上每个函数声明处前都应当加上注释, 描述函数的功能和用途. 只有在函数的功能简单而明显时才能省略这些注释(例如,  简单的取值和设值函数).  注释只是为了描述函数, 而不是命令函数做什么. 通常, 注释不会描述函数如何工作. 那是函数定义部分的事情.

函数声明处注释的模板:

```
/**
 *	@brief	这里添加函数作用描述
 *	@param	src	添加对src参数的描述
 *	@param	target	添加对target参数的描述
 *	@return	添加对返回值的描述
*/
bool detectArmor(const cv::Mat & src, Armor & target);
```

####  2.4.2 变量注释

**总述**

通常变量名本身足以很好说明变量用途. 某些情况下, 也需要额外的注释说明.

**说明**

变量的注释并没有太多的格式要求，主要需要注意排版，即对齐

```c++
double k;   // 空气阻力系数/质量
double g;   // 重力加速度
double bullet_speed;    // 弹速
double bs_coeff;        // 弹速系数
```

####  2.4.3 TODO注释

**总述**

`TODO`注释主要用于描述那些暂未完成的代码部分

```c++
/**
 *	TODO: 将来计划要做的事
*/
```

## Acknowledgements

​	我们参考了[阮一峰的网络日志](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)中关于git commit message的部分以及2023年IRobot视觉组代码规范部分。
