---
title: "PHP 文档编写标准"
post_status: publish
comment_status: open
taxonomy:
  category:
    - wpcs-docs
  post_tag:
    - Inline Documentation Standards
    - Repos
    - Data
---

# PHP 文档编写标准

WordPress 采用了一套定制化的文档规范，其灵感来源于 PHPDoc——一个由 [phpDocumentor](http://phpdoc.org/) 维护的、不断发展的 PHP 代码文档标准。

## 应记录的内容

WordPress 中的 PHP 文档主要采用格式化文档块或内联注释的形式。

以下是 WordPress 文件中应记录的内容列表：

- 函数和类方法
- 类
- 类成员（包括属性和常量）
- 引入和包含文件
- 钩子（动作和过滤器）
- 内联注释
- 文件头部
- 常量

### 文档编写技巧

#### 语言规范

摘要应清晰、简洁、简明。避免描述元素存在的"原因"，而应着重记录其"作用"和"触发时机"。

函数、钩子、类或方法属于**第三人称单数**元素，这意味着应使用**第三人称单数动词**来描述其功能。

[tip]
需要帮助记忆第三人称单数动词的变位规则吗？想象在函数、钩子、类或方法摘要前加上"它"：

- **正确示例**："（它）执行某项操作。"
- **错误示例**："（它）执行某项操作。"

[/tip]

摘要示例：

- **函数**：该函数**实现什么功能**？
    - 正确示例：_显示文章的最后修改日期。_
    - 错误示例：_显示文章最后被修改的日期。_
- **过滤器**：**过滤什么内容**？
    - 正确示例：_过滤文章内容。_
    - 错误示例：_允许您编辑文章模板中输出的文章内容。_
- **动作**：动作**何时触发**？
    - 正确示例：_在核心模块大部分加载完成且用户通过验证后触发。_
    - 错误示例：_在WordPress核心大量加载后，允许您注册自定义文章类型、自定义分类法并执行其他常规维护任务。_

#### 语法规范

描述性内容应使用完整句子撰写。此标准唯一的例外是文件头摘要，其定位更接近于文件"标题"而非完整句子。

在摘要、描述、参数或返回值说明中列举多个元素时，应使用序列（牛津）逗号。

#### 杂项

**`@since`**：在 WordPress 中查找某项功能被添加的版本时，推荐使用 [`svn blame`](http://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate)。对于较旧的钩子，另一个资源是 [WordPress Hooks Database](http://adambrown.info/p/wp_hooks)。

如果使用这些工具后仍无法确定版本号，请使用 `@since Unknown`。

任何从 WPMU 移植过来的内容应使用 `@since MU (3.0.0)`。现有的 `@since MU (3.0.0)` 标签不应更改。

**代码重构**：允许调整所记录的具体操作或过滤器行的间距以符合编码标准，但不要重构文件中的其他代码。

### 格式规范

[info]
WordPress 的 PHP 内联文档标准专门针对[官方代码参考](https://developer.wordpress.org/reference/)的最佳输出而定制。因此，遵循核心标准及下述格式规范对于确保预期输出结果**极其**重要。
[/info]

#### 通用规范

文档块应直接位于钩子、操作、函数、方法或类声明行之前。文档块与声明之间不应存在任何开闭标签或其他内容，以防止解析器产生混淆。

#### 摘要

摘要中不应使用任何 HTML 标记或 Markdown 格式。若文本提及 HTML 元素或标签，应写作“图像标签”或“img”元素，而非“`<img>`”。例如：

- 正确：_在页眉中打印链接标签时触发。_
- 错误：_在页眉中打印 `<link>` 标签时触发。_

允许使用行内 PHPDoc 标签。

#### 描述

除代码示例外，不应使用 HTML 标记，但描述中可根据需要使用 Markdown。

1. 列表：

    使用连字符 (-) 创建无序列表，前后各留一个空行。

    ```php
     * 包含无序列表的描述：
     *
     * - 这是项目 1。
     * - 这是项目 2。
     * - 这是项目 3。
     *
     * 描述继续...
    ```

    使用数字创建有序列表，前后各留一个空行。

    ```php
     * 包含有序列表的描述：
     *
     * 1. 这是项目 1。
     * 2. 这是项目 2。
     * 3. 这是项目 3。
     *
     * 描述继续...
    ```

2. 代码示例应通过将每行代码缩进 4 个空格来创建，前后各留一个空行。代码示例中的空行也需要缩进 4 个空格。请注意，以这种方式添加的示例将输出在 `<pre>` 标签中，并且*不会*进行语法高亮。

    ```php
     * 包含代码示例的描述：
     *
     *    $status = array(
     *        'draft'   => __( '草稿' ),
     *        'pending' => __( '待审核' ),
     *        'private' => __( '私密' ),
     *        'publish' => __( '已发布' )
     *    );
     *
     * 描述继续...
    ```

3. 以 URL 形式存在的链接，例如相关的 Trac 工单或其他文档，应使用 `@link` 标签添加到 DocBlock 中的适当位置：

    ```php
     * 描述文本。
     *
     * @link https://core.trac.wordpress.org/ticket/20000
    ```

#### `@since` 章节（更新日志）

每个函数、钩子、类和方法的文档都应包含对应的 `@since` 版本号（下文将详细说明）。

在 `@since` 标签的描述中不应使用 HTML，但必要时可以使用有限的 Markdown 语法，例如为变量、参数或参数名添加反引号，如 `$variable`。

版本号应采用三位数 `x.x.x` 格式：

```php
 * @since 4.4.0
```

如果对函数、钩子、类或方法进行了重大更改，应添加额外的 `@since` 标签、版本号和描述，以提供该函数的更新日志。

“重大更改”包括但不限于：

- 添加新参数。
- 必需参数变为可选。
- 更改默认/预期行为。
- 函数或方法成为新 API 的包装器。
- 参数被重命名（在宣布支持 PHP 8.0 后）。

PHPDoc 支持在 DocBlocks 中添加多个 `@since` 版本号正是出于此目的。在 `@since` 块中添加更新日志条目时，应注明版本号，并以句子形式添加描述，以句号结尾：

```php
 * @since 3.0.0
 * @since 3.8.0 添加了 `post__in` 参数。
 * @since 4.1.0 `$force` 参数现在为可选。
```

#### 其他描述

`@param`、`@type`、`@return`：这些标签的描述中不应使用 HTML，但必要时可使用有限的 Markdown，例如在变量周围添加反引号，如 `$variable`。

- 也可使用内联 `@see` 标签自动链接核心钩子：
    - 钩子，例如 `{@see 'save_post'}`
    - 动态钩子，例如 `{@see '$old_status_to_$new_status'}`（注意引号内的额外花括号已被移除）
- 默认值或可用值应使用单引号，例如 'draft'。可翻译字符串应在描述中注明。
- HTML 元素和标签应写作 "audio element" 或 "link tag"。

#### 行换行

文档块文本应在超过 80 个字符后换行至下一行。若文档块本身在左侧缩进 20 个字符位置，换行可在第 100 个字符位置发生，但总宽度不应超过 120 个字符。

## 文档块格式规范

以下各节中的示例展示了预期的文档块内容、标签及其精确格式要求。请使用空格而非制表符填充文档块，并确保每个标签组中的项目按示例所示对齐。

### 1. 函数与类方法

函数和类方法的格式应如下所示：

- **摘要**：对函数用途的简要说明，一句话，最多两行。末尾使用句号。
- **描述**：对摘要的补充，提供更详细的描述。句子末尾使用句号。
- **`@ignore`**：当某个元素仅用于内部使用且应跳过解析时使用。
- **`@since x.x.x`**：应始终为 3 位数字（例如 `@since 3.9.0`）。例外是 `@since MU (3.0.0)`。
- **`@access`**：仅用于核心专用函数或实现“私有”核心 API 的类。如果元素是私有的，输出时会附带一条说明其用于内部用途的消息。
- **`@see`**：引用函数、方法或类内部严重依赖的内容。关于预期格式，请参阅上文关于内联 `@see` 标签的说明。
- **`@link`**：提供更多信息的 URL。此标签绝不应用于引用另一个函数、钩子、类或方法，请使用 `@see`。
- **`@global`**：列出在函数或方法中使用的 PHP 全局变量，可附带对全局变量的描述。如果列出多个全局变量，它们应按类型、变量和描述作为一组进行对齐。
- **`@param`**：在描述前注明参数是否为 _可选_，并在末尾包含句号。描述应提及可接受的值以及默认值。例如：_可选。此值执行某些操作。接受 'post'、'term' 或空值。默认为空。_
- **`@return`**：应包含所有可能的返回类型及每种类型的描述。末尾使用句号。注意：`@return void` 不应在默认捆绑主题和 WordPress 核心中包含的 PHP 兼容性垫片之外使用。

```php
/**
 * 摘要。
 *
 * 描述。
 *
 * @since x.x.x
 *
 * @see 所依赖的函数/方法/类
 * @link URL
 * @global 类型 $变量名 描述。
 * @global 类型 $变量名 描述。
 *
 * @param 类型 $变量 描述。
 * @param 类型 $变量 可选。描述。默认值。
 * @return 类型 描述。
 */
```

#### 1.1 Parameters That Are Arrays

Parameters that are an array of arguments should be documented in the "originating" function only, and cross-referenced via an `@see` tag in corresponding DocBlocks.

Array values should be documented using WordPress' flavor of hash notation style similar to how [Hooks](https://developer.wordpress.org/coding-standards/inline-documentation-standards/php/#4-hooks-actions-and-filters) can be documented, each array value beginning with the `@type` tag, and taking the form of:

```php
*     @type type $key Description. Default 'value'. Accepts 'value', 'value'.
*                     (aligned with Description, if wraps to a new line)
```

An example of an "originating" function and re-use of an argument array is [`wp_remote_request|post|get|head()`](https://core.trac.wordpress.org/browser/tags/6.0/src/wp-includes/http.php/#L114).

```php
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @param type  $var Description.
 * @param array $args {
 *     Optional. An array of arguments.
 *
 *     @type type $key Description. Default 'value'. Accepts 'value', 'value'.
 *                     (aligned with Description, if wraps to a new line)
 *     @type type $key Description.
 * }
 * @param type  $var Description.
 * @return type Description.
 */
```

In most cases, there is no need to mark individual arguments in a hash notation as _optional_, as the entire array is usually optional. Specifying "Optional." in the hash notation description should suffice. In the case where the array is NOT optional, individual key/value pairs may be optional and should be marked as such as necessary.

#### 1.2 已弃用的函数

如果函数已被弃用且不应再使用，则应添加 `@deprecated` 标签，并附带版本号以及应使用何种替代方案的描述。请注意额外使用了 `@see` 标签——代码参考会利用此信息尝试链接到替代函数。

```php
/**
 * 摘要。
 *
 * 描述。
 *
 * @since x.x.x
 * @deprecated x.x.x 请使用 new_function_name()
 * @see new_function_name()
 *
 * @param type $var 可选。描述。
 * @param type $var 描述。
 * @return type 描述。
 */
```

### 2. 类

类的文档块应遵循以下格式：

- **摘要**：用一句话简要说明该类的**用途**，最多不超过两行。结尾使用句号。
- **描述**：对摘要的补充，提供更详细的说明。结尾使用句号。
- **`@since x.x.x`**：应始终为三位数（例如 `@since 3.9.0`）。例外情况是 `@since MU (3.0.0)`。

```php
/**
 * 摘要。
 *
 * 描述。
 *
 * @since x.x.x
 */
```

如果记录的是子类，包含一个指向父类的 `@see` 标签引用也很有帮助：

```php
/**
 * 摘要。
 *
 * 描述。
 *
 * @since x.x.x
 *
 * @see Super_Class
 */
```

#### 2.1 类成员

##### 2.1.1 属性

类属性应按以下格式编写：

- **摘要**：末尾使用句号。
- **`@since x.x.x`**：应始终为 3 位数字（例如 `@since 3.9.0`）。例外是 `@since MU (3.0.0)`。
- **`@var`**：格式与 `@param` 相同，但描述可以省略。

```php
/**
 * 摘要。
 *
 * @since x.x.x
 * @var type $var 描述。
 */
```

##### 2.1.2 常量

- **摘要**：末尾使用句号。</li>
- **`@since x.x.x`**：应始终为 3 位数字（例如 `@since 3.9.0`）。例外是 `@since MU (3.0.0)`。
- **`@var`**：格式与 `@param` 相同，但描述可以省略。

```php
/**
 * 摘要。
 *
 * @since x.x.x
 * @var type $var 描述。
 */
const NAME = value;
```

### 3. 引入与包含文件

被引入或包含的文件应使用摘要描述性文档块进行注释。为求清晰，此规则也可酌情应用于内联的 `get_template_part()` 调用。

```php
/**
 * 摘要。
 */
require_once( ABSPATH . WPINC . '/filename.php' );
```

### 4. Hooks (Actions and Filters)

Both action and filter hooks should be documented on the line immediately preceding the call to `do_action()` or `do_action_ref_array()`, or `apply_filters()` or `apply_filters_ref_array()`, and formatted as follows:

- **Summary**: A brief, one line explanation of the purpose of the hook. Use a period at the end.
- **Description**: A supplemental description to the summary, if warranted.
- **`@ignore`**: Used when a hook is meant only for internal use and should be skipped from parsing.
- **`@since x.x.x`**: Should always be 3-digit (e.g. `@since 3.9.0`). Exception is `@since MU (3.0.0)`.
- **`@param`**: If the parameter is an array of arguments, document each argument using a hash notation (described above in the _Parameters That Are Arrays_ section), and include a period at the end of each line.

Note that `@return` is _not_ used for hook documentation, because action hooks return nothing, and filter hooks always return their first parameter.

```php
/**
 * Summary.
 *
 * Description.
 *
 * @since x.x.x
 *
 * @param type  $var Description.
 * @param array $args {
 *     Short description about this hash.
 *
 *     @type type $var Description.
 *     @type type $var Description.
 * }
 * @param type  $var Description.
 */
```

If a hook is in the middle of a block of HTML or a long conditional, the DocBlock should be placed on the line immediately before the start of the HTML block or conditional, even if it means forcing line-breaks/PHP tags in a continuous line of HTML.

Tools to use when searching for the version a hook was added are [`svn blame`](http://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate), or the [WordPress Hooks Database](http://adambrown.info/p/wp_hooks) for older hooks. If, after using these tools, the version number cannot be determined, use `@since Unknown`.

#### 4.1 重复的钩子

有时，同一个钩子会在相同或不同的核心文件中被多次使用。在这种情况下，无需每次都列出完整的 DocBlock，只需对首次添加或逻辑上最合适位置的动作或过滤器进行完整文档记录。后续版本应使用单行注释。

对于动作：

```php
/** 此动作的文档位于 path/to/filename.php */
```

对于过滤器：

```php
/** 此过滤器的文档位于 path/to/filename.php */
```

要确定应记录哪个实例，请先搜索相同钩子标签的多个使用位置，然后使用 [`svn blame`](http://make.wordpress.org/core/handbook/svn/code-history/#using-subversion-annotate) 查找最早修订版本中首次使用的钩子。如果同一版本中添加了多个钩子实例，则将逻辑上最合适的位置记录为“主要”实例。

### 5. 行内注释

方法或函数内部的行内注释应遵循以下格式：

#### 5.1 单行注释

```php
// 允许插件过滤数组。
```

#### 5.2 多行注释

```php
/*
 * 这是一个足够长的注释，需要跨越多行。
 * 您会注意到，其格式基本上与 PHPDoc 的包装和注释块风格相同。
 */
```

**重要提示**：多行注释不能以 `/**`（双星号）开头，因为解析器可能会将其误认为是 DocBlock。请改用 `/*`（单星号）。

### 6. 文件头部

文件头部文档块用于概述文件包含的内容。

只要可能，**所有** WordPress 文件都应包含头部文档块，无论文件内容如何——这包括包含类的文件。

```php
/**
 * 摘要（文件头部不使用句号）
 *
 * 描述。（使用句号）
 *
 * @link URL
 *
 * @package WordPress
 * @subpackage Component
 * @since x.x.x（文件引入的版本）
 */
```

_摘要_ 部分旨在简洁地描述文件服务的**具体**用途。

示例：

- 良好：_"Widgets API: WP_Widget 类"_
- 不佳：_"核心小工具类"_

_描述_ 部分可用于更好地解释文件的概述信息，例如特定文件如何融入 API 或组件的整体结构。

示例：

- 良好：_"Widgets API 由 WP_Widget 和 WP_Widget_Factory 类以及一系列实现小工具和相关侧边栏 API 的顶层功能组成。WordPress 默认注册了许多常用小工具。"_

### 7. 常量

常量文档块用于描述常量，以便更好地使用和理解。

常量应按以下格式编写：

- **摘要**：末尾使用句号。
- **`@since x.x.x`**：应始终为 3 位数字（例如 `@since 3.9.0`）。例外情况是 `@since MU (3.0.0)`。
- **`@var`**：格式与 `@param` 相同。描述是可选的。

```php
/**
 * 摘要。
 *
 * @since x.x.x (如果可用)
 * @var 类型 $变量 描述。
 */
```

## PHPDoc 标签

WordPress 中常用的 PHPDoc 标签包括 `@since`、`@see`、`@global`、`@param` 和 `@return`（完整列表见下表）。

大多数情况下标签使用正确，但并非总是如此。例如，有时会看到行内使用 `@link` 标签来链接到单独的函数或方法。对于已知的类、方法或函数进行“链接”是不必要的，因为代码参考会自动链接这些元素。对于行内“链接”钩子，应使用 `@see` 标签——详见《其他描述》部分。

| Tag               | Usage                                                       | Description                                                                                                                                                                                                                                    |
|-------------------|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **`@access`**     | private                                                     | Only used in limited circumstances, like when visibility modifiers cannot be used in the code, and only when private, such as for core-only functions or core classes implementing "private" APIs. Used directly below the `@since` line in block.                                                        |
| **`@deprecated`** | version x.x.x Use _replacement function name_ instead       | What version of WordPress the function/method was deprecated. Use 3-digit version number. Should be accompanied by a matching `@see` tag.                                                                                                      |
| **`@global`**     | datatype $variable description                              | Document global(s) used in the function/method. For boolean and integer types, use `bool` and `int`, respectively.                                                                                                                             |
| **`@internal`**   | information string                                          | Typically used wrapped in `{}` for adding notes for internal use only.                                                                                                                                                                                         |
| **`@ignore`**     | (standalone)                                                | Used to skip parsing of the entire element.                                                                                                                                                                                                    |
| **`@link`**       | URL                                                         | Link to additional information for the function/method. For an external script/library, links to source. Not to be used for related functions/methods; use `@see` instead.                                                                     |
| **`@method`**     | returntype description                                      | Shows a "magic" method found inside the class.                                                                                                                                                                                                 |
| **`@package`**    | packagename                                                 | Specifies package that all functions, includes, and defines in the file belong to. Found in DocBlock at top of the file. For core (and bundled themes), this is always **WordPress**.                                                          |
| **`@param`**      | datatype $variable description                              | Function/method parameter of the format: parameter type, variable name, description, default behavior. For boolean and integer types, use `bool` and `int`, respectively.                                                                      |
| **`@return`**     | datatype description                                        | Document the return value of functions or methods. `@return void` should not be used outside of the default bundled themes. For boolean and integer types, use `bool` and `int`, respectively.                                                 |
| **`@see`**        | elementname                                                 | References another function/method/class the function/method relies on. Should only be used inline for "linking" hooks.                                                                                                                        |
| **`@since`**      | version x.x.x                                               | Documents release version function/method was added. Use 3-digit version number - this is to aid with version searches, and for use when comparing versions in code. Exception is `@since MU (3.0.0)`.                                         |
| **`@static`**     | (standalone)                                                | Note: This tag has been used in the past, but should no longer be used. Just using the static keyword in your code is enough for phpDocumentor on PHP5+ to recognize static variables and methods, and PhpDocumentor will mark them as static. |
| **`@staticvar`**  | datatype $variable description                              | Note: This tag has been used in the past, but should no longer be used. Document a static variable's use in a function/method. For boolean and integer types, use `bool` and `int`, respectively.                                              |
| **`@subpackage`** | subpackagename                                              | For page-level DocBlock, specifies the Component that all functions and defines in file belong to. For class-level DocBlock, specifies the subpackage/component the class belongs to.                                                          |
| **`@todo`**       | information string                                          | Documents planned changes to an element that have not been implemented.                                                                                                                                                                        |
| **`@type`**       | datatype description for an argument array value            | Used to denote argument array value types. See the **Hooks** or **Parameters That Are Arrays** sections for example syntax.                                                                                                                    |
| **`@uses`**       | class::methodname() / class::$variablename / functionname() | Note: This tag has been used in the past, but should no longer be used. References a key function/method used. May include a short description.                                                                                                |
| **`@var`**        | datatype description                                        | Data type for a class variable and short description. Callbacks are marked callback.                                                                                                                                                           |

[info]
PHPDoc 标签可与某些文本编辑器/IDE 配合使用，以显示关于代码段的更多信息。这对于使用这些编辑器的开发者理解代码的用途以及在何处使用它非常有用。PhpStorm 和 Netbeans 已支持 PHPDoc。

以下文本编辑器/IDE 有可安装的扩展/插件包，可帮助您自动创建 DocBlocks：

- Notepad++: [DocIt for Notepad++](http://sourceforge.net/projects/nppdocit/) (Windows)
- TextMate: [php.tmbundle](https://github.com/textmate/php.tmbundle) (Mac)
- SublimeText: [sublime packages](https://packagecontrol.io/search/phpdoc) (Windows, Mac, Linux)

注意：即使有生成 DocBlocks 的帮助，大多数代码编辑器的工作并不十分彻底——您可能需要手动填写任何生成的 DocBlocks 的某些部分。
[/info]

### 已弃用标签

> **前言：** 目前，为了保持一致性，WordPress 核心将继续使用 `@subpackage` 标签——无论是在编写新的 DocBlocks 时，还是在编辑旧的 DocBlocks 时。
>
> 只有当新的——外部的——PSR-5 建议最终确定后，才会考虑进行全面的更改，例如弃用某些标签。

根据[新的 PSR-5](https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md) 建议，以下 PHPDoc 标签应被弃用：

- `@subpackage`（推荐使用统一的包标签：`@package Package\Subpackage`）
- `@static`（不再需要）
- `@staticvar`（不再需要）

### 其他标签

**插件与主题中的 `@package` 标签（捆绑主题除外）**

第三方插件和主题作者**不得**在其插件或主题中使用 `@package WordPress`。插件的 `@package` 名称应为插件名称；主题的 `@package` 名称应为主题名称，并使用下划线分隔：例如 `Twenty_Fifteen`。

**`@author` 标签**

WordPress 的政策是不使用 `@author` 标签，除非在维护外部库时保留它。我们不希望暗示任何形式的代码“所有权”，以免阻碍贡献。

**`@copyright` 与 `@license` 标签**

`@copyright` 和 `@license` 标签用于外部库和脚本，不应在 WordPress 核心文件中使用。

- `@copyright` 用于指定外部脚本的版权信息。
- `@license` 用于指定外部脚本的许可证信息。

## Resources

- [Wikipedia on PHPDoc](http://en.wikipedia.org/wiki/PHPDoc)
- [PEAR Standards](http://pear.php.net/manual/en/standards.sample.php)
- [phpDocumentor](http://www.phpdoc.org/)
- [phpDocumentor Tutorial Tags](http://manual.phpdoc.org/HTMLSmartyConverter/HandS/phpDocumentor/tutorial_tags.pkg.html)
- [Draft PSR-5 recommendations](https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc.md)
- [Draft PSR-19 recommendations](https://github.com/phpDocumentor/fig-standards/blob/master/proposed/phpdoc-tags.md)